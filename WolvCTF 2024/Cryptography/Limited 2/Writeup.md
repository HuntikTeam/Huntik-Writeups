Limited 2
=============

## Description

![Description](./Limited%202.png)

## Solution

We are given `NY_chal_time` :

```python
import time
import random
import sys

if __name__ == '__main__':
    flag = input("Flag? > ").encode('utf-8')
    correct = [192, 123, 40, 205, 152, 229, 188, 64, 42, 166, 126, 125, 13, 187, 91]
    if len(flag) != len(correct):
        print('Nope :(')
        sys.exit(1)
    if time.gmtime().tm_year >= 2024 or time.gmtime().tm_year < 2023:
        print('Nope :(')
        sys.exit(1)
    if time.gmtime().tm_yday != 365 and time.gmtime().tm_yday != 366:
        print('Nope :(')
        sys.exit(1)    
    for i in range(len(flag)):
        # Totally not right now
        time_current = int(time.time())
        random.seed(i+time_current)
        if correct[i] != flag[i] ^ random.getrandbits(8):
            print('Nope :(')
            sys.exit(1)
        time.sleep(random.randint(1, 60))
    print(flag)
```

From the given script we know that the seed is somewhere between the epoch time of the 365 and 366 day of 2023 
So we just bruteforce the seeds and replicate the algorithm

```python
import time
import random
import sys

correct = [192, 123, 40, 205, 152, 229, 188, 64, 42, 166, 126, 125, 13, 187, 91]

# Define the year and the day of the year
year = 2023
day_of_year = [365, 366]

# Initialize the time structure
time_struct = time.struct_time((year, 1, 1, 0, 0, 0, 0, 1, -1))

# Calculate the start and end times for each day of the year
for day in day_of_year:
    start_time = time.mktime(time_struct) + (day - 1) * 24 * 60 * 60
    end_time = start_time + 24 * 60 * 60 - 1
    print(f"For day {day} of {year}, the range in time.time() format is [{start_time}, {end_time}]")

# 1703998800 - 1704171599
#1704171600 + 60 = 1704171660
#1703826001

match = 0
for j in range(1703998800,1704171600):

    flag = []
    seed = j
    for i in range(len(correct)):
        random.seed(seed)
        flag.append(correct[i] ^ random.getrandbits(8))
        seed = seed + random.randint(1, 60) + 1

    strFlag = ""
    for i in flag:
        strFlag = strFlag + chr(i)

    if "wctf" in strFlag:
        print(strFlag)

#wctf{b4ll_dr0p}
```
## Flag

`wctf{b4ll_dr0p}`

