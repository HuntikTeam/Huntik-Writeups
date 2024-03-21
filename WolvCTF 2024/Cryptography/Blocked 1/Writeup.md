Blocked 1
=============

## Description

![Description](./Blocked%201.png)

## Solution

We are given `blocked 1` :

```python

"""
----------------------------------------------------------------------------
NOTE: any websites linked in this challenge are linked **purely for fun**
They do not contain real flags for WolvCTF.
----------------------------------------------------------------------------
"""

import random
import secrets
import sys
import time

from Crypto.Cipher import AES


MASTER_KEY = secrets.token_bytes(16)


def generate(username):
    iv = secrets.token_bytes(16)
    msg = f'password reset: {username}'.encode()
    if len(msg) % 16 != 0:
        msg += b'\0' * (16 - len(msg) % 16)
    cipher = AES.new(MASTER_KEY, AES.MODE_CBC, iv=iv)
    return iv + cipher.encrypt(msg)


def verify(token):
    iv = token[0:16]
    msg = token[16:]
    cipher = AES.new(MASTER_KEY, AES.MODE_CBC, iv=iv)
    pt = cipher.decrypt(msg)
    username = pt[16:].decode(errors='ignore')
    return username.rstrip('\x00')


def main():
    username = f'guest_{random.randint(100000, 999999)}'

    print("""                 __      __
 _      ______  / /___  / /_ _   __
| | /| / / __ \\/ / __ \\/ __ \\ | / /
| |/ |/ / /_/ / / /_/ / / / / |/ /
|__/|__/\\____/_/ .___/_/ /_/|___/
              /_/""")
    print("[      password reset portal      ]")
    print("you are logged in as:", username)
    print("")
    while True:
        print(" to enter a password reset token, please press 1")
        print(" if you forgot your password, please press 2")
        print(" to speak to our agents, please press 3")
        s = input(" > ")
        if s == '1':
            token = input(" token > ")
            if verify(bytes.fromhex(token)) == 'doubledelete':
                print(open('flag.txt').read())
                sys.exit(0)
            else:
                print(f'hello, {username}')
        elif s == '2':
            print(generate(username).hex())
        elif s == '3':
            print('please hold...')
            time.sleep(2)
            # thanks chatgpt
            print("Thank you for reaching out to WOLPHV customer support. We appreciate your call. Currently, all our agents are assisting other customers. We apologize for any inconvenience this may cause. Your satisfaction is important to us, and we want to ensure that you receive the attention you deserve. Please leave your name, contact number, and a brief message, and one of our representatives will get back to you as soon as possible. Alternatively, you may also visit our website at https://wolphv.chal.wolvsec.org/ for self-service options. Thank you for your understanding, and we look forward to assisting you shortly.")
            print("<beep>")


main()

```

The technique here is bit flipping on the AES CBC.It took me a while to figure it out
But the idea is basically to manipulate the bits of the previous block of the cryptotext in order to force the decoding to the needed string doubledelete

```python
from pwn import *

conn = remote('blocked1.wolvctf.io', 1337)

conn.recvuntil(b'as: ')
user = conn.recvline().decode().strip("\r\n")
print(user)

while(1):

	conn.recvuntil(b'> ')
	conn.sendline(b'2')
	hexToken = conn.recvline().decode().strip("\r\n")
	print(hexToken)

	byteToken = bytes.fromhex(hexToken)

	iv = byteToken[0:16]

	block1 = byteToken[16:32]
	block2 = byteToken[32:]

	originalMsg   = user.encode()
	targetMessage = b'doubledelete'

	flipped_bits = xor(targetMessage,originalMsg)

	partialBlock1 = block1[:12]
	finalBlock1 = xor(partialBlock1,flipped_bits)
	finalBlock1 = finalBlock1 + block1[12:]

	finalMessage = iv + finalBlock1 + block2

	newToken = str(finalMessage.hex()).encode()

	print(newToken)

	conn.recvuntil(b'> ')
	conn.sendline(b'1')
	print(conn.recvuntil(b'token > '))

	conn.sendline(newToken)
	print(conn.recvline())

conn.close()

#wctf{th3y_l0st_th3_f1rst_16_byt35_0f_th3_m3ss4g3_t00}
```

## Flag

`wctf{th3y_l0st_th3_f1rst_16_byt35_0f_th3_m3ss4g3_t00}`

