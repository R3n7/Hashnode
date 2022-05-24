## Factor Master - TJCTF 2022

As the name suggests , We have to factorize numbers.
This challenge is based on factoring n on RSA.
This is the server side script for the challenge.
```py
#!/usr/local/bin/python -u

from Crypto.Util.number import getPrime, isPrime, getRandomInteger
import sys, random

print("Are you truly the master here?")
print()
print("We'll have to find out...")
print()

def fail():
    print("You are not the master!")
    sys.exit(1)

def challenge1():
    p = getPrime(44)
    q = getPrime(1024)
    n = p * q
    return [p, q], n

def challenge2():
    p = getPrime(1024)
    q = p + getRandomInteger(524)
    if q % 2 == 0: q += 1
    while not isPrime(q): q += 2
    n = p * q
    return [p, q], n

def challenge3():
    small_primes = [n for n in range(2,10000) if isPrime(n)]
    def gen_smooth(N):
        r = 2
        while True:
            p = random.choice(small_primes)
            if (r * p).bit_length() > N:
                return r
            r *= p
    p = 1
    while not isPrime(p):
        p = gen_smooth(1024) + 1
    q = getPrime(1024)
    n = p * q
    return [p, q], n

challenges = [challenge1, challenge2, challenge3]
responses = ["Okay, not bad.", "Nice job.", "Wow."]

for i, chal in enumerate(challenges):
    print(f"CHALLENGE {i+1}")
    factors, n = chal()
    factors.sort()
    print(f"n = {n}")
    guess = input("factors = ? ")
    if guess != " ".join(map(str,factors)):
        fail()
    print(responses[i])
    print()

print("Here's your flag:")
with open("flag.txt") as f:
    print(f.read().strip())
```

<br>
First we connect to the server. Then we are greeted with the first challenge , we are given n. we have to find p,q . If we see the challenge1 function on server,we can find that q is 1024 bits but p is relatively small it is only 44 bits. So we can brute the small prime.<br>
Fortunately sage does this for us.<br>
```
prime_factors(n)
```
We get the prime factors.now for the second challenge,we are given primes that are close to each other. so we can try Fermat factorization to factorize.<br>
[Fermat_Factor](https://github.com/rameshgkwd05/fermat_factorization/blob/master/fermat_factor.sage)<br>
```py
def fermat_factor(n):
    if is_even(n):
        n2 = n/2
        return n2,2
    
    if is_prime(n):
        return 1,n
    
    a = ceil(sqrt(n))
    b2 = a*a - n
    
    while(is_square(b2) == False):
        a +=1
        b2 = a*a - n

    b = floor(sqrt(b2))

    return (a-b),(a+b)
fermat_factor(n)
```
Next and final challenge is we are given a smooth prime + 1 as p. so we use pollard p-1 method to factorize n.<br>
[Pollard_pm1](https://sage.sagemath.org/share/public_paths/ca3d2afaf7d5e747d3d292997f5b8f2fcc704c34)<br>
```py
#https://sage.sagemath.org/share/public_paths/ca3d2afaf7d5e747d3d292997f5b8f2fcc704c34
def pollard_pm1(N,B=0):
    if not B: B=ceil(sqrt(N))
    a = Integers(N).random_element()
    b = a
    for ell in primes(B):
        q = 1
        while q < N:
            q *= ell
        b = b**q
        if b == 1:
            return 0
        d = gcd(b.lift()-1,N)
        if d > 1: return d
    return 0
pollard_pm1(n)
```
Bringing all those things together and creating pwntools script for sending and receiving information from the server.<br>
Script.py:<br>
```py
from pwn import *
from sage.all import *
from Crypto.Util.number import getPrime,isPrime,long_to_bytes,inverse
#https://sage.sagemath.org/share/public_paths/ca3d2afaf7d5e747d3d292997f5b8f2fcc704c34
def pollard_pm1(N,B=0):
    if not B: B=ceil(sqrt(N))
    a = Integers(N).random_element()
    b = a
    for ell in primes(B):
        q = 1
        while q < N:
            q *= ell
        b = b**q
        if b == 1:
            return 0
        d = gcd(b.lift()-1,N)
        if d > 1: return d
    return 0
#https://github.com/rameshgkwd05/fermat_factorization/blob/master/fermat_factor.sage
def fermat_factor(n):
    if is_even(n):
        n2 = n/2
        return n2,2
    
    if is_prime(n):
        return 1,n
    
    a = ceil(sqrt(n))
    b2 = a*a - n
    
    while(is_square(b2) == False):
        a +=1
        b2 = a*a - n

    b = floor(sqrt(b2))

    return (a-b),(a+b)
r = remote('tjc.tf',31782)
print(r.recvuntil("CHALLENGE 1"))
n = r.recvuntil("? ").decode().split("\n")[1]
n = int(n[3:])
factors = prime_factors(n)
f1= " ".join(map(str,factors))
r.sendline(f1.encode())
n = r.recvuntil("? ").decode().split("\n")[3]
n = int(n[3:])
factors = fermat_factor(n)
f2 = " ".join(map(str,factors))
r.sendline(f2.encode())
n = r.recvuntil("? ").decode().split("\n")[3]
n = int(n[3:])
factors = [ ]
p = pollard_pm1(n)
factors.append(p)
factors.append(n//p)
f3 = " ".join(map(str,factors))
r.sendline(f3.encode())
print(r.recvuntil("}"))
#Here's your flag:\ntjctf{S0_y0u_r34lly_c4n_F4c7t0r_4ny7th1nG_c36f63cfe73c}
```
PS:
If you run these functions on python it won't work . import sage.all module and then run.