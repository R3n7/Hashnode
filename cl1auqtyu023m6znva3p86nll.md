## Babyrevs - Wolvseccon 2022

BabyRev0
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648315818194/LrJsjetsF.png)

Downloading The file Babyre0 and Running Strings Gives us the flag.
<br>
Flag:`wsc{juST_a_b4By_RE!}`

BabyRev1

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648316226454/ex2pjFpti.png)
Downloading The file Babyre1 and Running it on IDA.
We Look at the encode function. it is xoring every element in flag with 59.<br>
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648316363736/jJif2-kkg.png)
<br>
These are Xorred with 59. So if we XOR them Again with 59. we get the flag.
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648316523415/ZwEiVzs1Q.png)
```py
from pwn import xor
x = ['4c', '48', '58', '40', '62', '0b', '4e', '64', '', '0f', '49', '08', '64', '5c', '08', '4f', '4f', '0a', '55', '5c', '64', '4f', '53', '08', '64', '53', '0f', '55', '5c', '64', '0b', '5d', '64', '6f', '73', '72', '68', '1a', '46', '47', '43']
for i in x:
  print(xor(bytes.fromhex(i),chr(59).encode()).decode(),end='')
```
Flag:`wsc{Y0u_;4r3_g3tt1ng_th3_h4ng_0f_THIS!}`