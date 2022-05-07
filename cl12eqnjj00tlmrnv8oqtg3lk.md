## JumbleBumble - VishwaCTF 2022

Challenge Info:<br>


![2022-03-22 22_49_41-VishwaCTF — Mozilla Firefox.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1647969616383/obLC1CPvM.png)
Description: Jumble Bumble been encode, get the flag from the code
<br>
Files: ` Script.py , JumbleBumble.txt`<br>
The `Script.py` file:
<br>
```py
import random
from Crypto.Util.number import getPrime, bytes_to_long

flags = []

with open('stuff.txt', 'rb') as f:
    for stuff in f.readlines():
        flags.append(stuff)


with open('flag.txt', 'rb') as f:
    flag = f.read()
    flags.append(flag)

random.shuffle(flags)

for rand in flags:
    p = getPrime(1024)
    q = getPrime(1024)
    n = p * q
    e = 4
    m = bytes_to_long(rand)
    c = pow(m, e, n)
    with open('output.txt', 'a') as f:
        f.write(f'{n}\n')
        f.write(f'{e}\n')
        f.write(f'{c}\n\n')
    
```
As we can see , there are random things added to the flags list.For every element in flags list we get new P,Q for encrypting the element. But the e is same and small for every element in the list. Now, computing the e-th root over the integers should work,  since  m^e < N , so their is no 'wrap around', i.e., \\[ m^e \bmod N  = m^e \\]
So the ciphertext is just m^e. we can take the root of ciphertext to get the message.
Script:<br>

```py
import gmpy
from Crypto.Util.number import long_to_bytes
f = open("1.txt","r")
f =f.read().split("\n")
list1 = [ ]
list2 =[ ]
count =0 
for i in range(len(f)):
    if f[i]!=" ":
        if count<3:
            list1.append(f[i])
            count+=1
        else:
            count=0
            list2.append(list1)
            list1=[ ]

flag = " "
for i in range(len(list2)):
    n = int(list2[i][0])
    e = int(list2[i][1])
    c =int(list2[i][2])
    m0=gmpy.mpz(c)
    flag+=str(long_to_bytes(m0.root(e)[0]))
print(flag)
``` 
So here we take every ciphertext from JumbleBumble file and take the 4th Root and convert them into bytes and concatenate them to the flag string.
Flag:`VishwaCTF{c4yp70gr2phy_1s_n07_e25y}`
