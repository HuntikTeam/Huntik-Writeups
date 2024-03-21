Limited 1
=============

## Description

![Description](./Limited%201.png)

## Solution

We are given `chal_time` :

```python
import time
import random
import sys

if __name__ == '__main__':
    flag = input("Flag? > ").encode('utf-8')
    correct = [189, 24, 103, 164, 36, 233, 227, 172, 244, 213, 61, 62, 84, 124, 242, 100, 22, 94, 108, 230, 24, 190, 23, 228, 24]
    time_cycle = int(time.time()) % 256
    if len(flag) != len(correct):
        print('Nope :(')
        sys.exit(1)
    for i in range(len(flag)):
        random.seed(i+time_cycle)
        if correct[i] != flag[i] ^ random.getrandbits(8):
            print('Nope :(')
            sys.exit(1)
    print(flag)
```

Flag is encrypted with XOR chosen randomly
But seed and the time cycle are easily bruteforcable as time cycle is mod 256

```python
import time
import random
import sys


correct = [189, 24, 103, 164, 36, 233, 227, 172, 244, 213, 61, 62, 84, 124, 242, 100, 22, 94, 108, 230, 24, 190, 23, 228, 24]

while(1):
    time_cycle = int(time.time()) % 256

    flag = []

    for i in range(len(correct)):
        random.seed(i+time_cycle)
        flag.append(correct[i] ^ random.getrandbits(8))

    #print(flag)

    strFlag = ""
    for i in flag:
        strFlag = strFlag + chr(i)

    if "wctf" in strFlag:
        print(strFlag)

    #print(strFlag)

#wctf{f34R_0f_m1ss1ng_0ut}
```
Running this for like 1-2 minutes gives the flag

## Flag

`wctf{f34R_0f_m1ss1ng_0ut}`
