# Rick'S Algorithm

## Description

My friend Rick designed an alogrithm that is super secure! Feel free to try it!

use [nc IP PORT]

Author: SKR

## Hint
Heard of modular arithmetic?

## Challenge File

```python

from Crypto.Util.number import *
import os
from secret import revealFlag
flag = bytes_to_long(b"wgmy{REDACTED}")

p = getStrongPrime(1024)
q = getStrongPrime(1024)
e = 0x557
n = p*q
phi = (p-1)*(q-1)
d = inverse(e,phi)

while True:
	print("Choose an option below")
	print("=======================")
	print("1. Encrypt a message")
	print("2. Decrypt a message")
	print("3. Print encrypted flag")
	print("4. Print flag")
	print("5. Exit")

	try:
		option = input("Enter option: ")
		if option == "1":
			m = bytes_to_long(input("Enter message to encrypt: ").encode())
			print(f"Encrypted message: {pow(m,e,n)}")
		elif option == "2":
			c = int(input("Enter ciphertext to decrypt: "))
			if c % pow(flag,e,n) == 0 or flag % pow(c,d,n) == 0:
				print("HACKER ALERT!!")
				break
			print(f"Decrypted message: {pow(c,d,n)}")
		elif option == "3":
			print(f"Encrypted flag: {pow(flag,e,n)}")
		elif option == "4":
			print("Revealing flag: ")
			revealFlag()
		elif option == "5":
			print("Bye!!")
			break
	except Exception as e:
		print("HACKER ALERT!!")
	
```
## Solution

Let's analyze the given code.
We are given RSA with standard params in the beginning.
The e = 1367 is a bit unusual but still nothing that could lead to a vulnerability.

The options for server communication we have are :

1) Encrypt any message
2) Decrypt any message that is not in any form related to the encrypted or decrypted flag
3) Give the encrypted flag
4) Rick Roll
5) Exit



![Description](../../../images/Credentials.png)

## Flag
`wgmy{ce7a475ff0e122e6ac34c3765449f71d}`
