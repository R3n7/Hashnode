## Daredevil's server - FoobarCTF 2022


![chall.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1646984645094/ad2YwBkkZ.png)
This was one of the Crypto Challenge in Foobar CTF 2022.When we connect to the server,we are greeted with these options.
![server_start.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1646984902217/D0rxMyCQd.png)
Lets see the Encryption Function
![eFunc.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1646985022376/AAzJEXjKI.png)
We are given options on a server to Sign and Verify. Sign will sign an integer using the RSA signature scheme and Verify checks if the signature entered is for the Message.Our challenge to is verify for Token message given to us.But the Sign Function won't sign anything with the Token Present. so we need to trick it into signing our message using an [RSA blinding attack](https://en.wikipedia.org/wiki/Blind_signature#Dangers_of_blind_signing).<br>
## **Blind Signatures**<br>
Blind signatures are signatures generated when the signer is supposed unaware of the contents of what they are signing.

To accomplish RSA blind signatures, first a random number r
is chosen that is relatively prime to N. This is the blinding factor. The blinded message M′ is then calculated by:


\\[
M^′=M×r^e \,mod\,N
\\]

where M is the original message, e is the public exponent, and N is the public modulus.
Then the blinded signature, S′ is calcuated by the signer:
\\[
S^′=M^{'d} \,mod\,N
\\]
Now the unblinded-signature can be calculated knowing r:
\\[S=\frac{S^′}{r}\, mod\,N\\]

The reason this works is because the signer is really just decrypting the message with their secret key, so we can say:
\\[M^{′d}=(M×r^e)^d\,mod\,N\\]
\\[M^{′d}=(M^d×r^{ed})\,mod\,N\\]

\begin{align} \text{Now the RSA decryption gives us that }  r^{ed} = r \bmod{N}  \text{ making our equation: }\end{align}
\\[M'^d = (M^d \times r) \bmod{N}\\]

and since the gcd(r,N)=1 we now are left with:
\\[S*r = M'^d \bmod{N}\\]
\\[S = \frac{M'^d}{r} \bmod{N}\\]
which brings us the equation from earlier:
\\[S = \frac{M'^d}{r} \bmod{N}\\]

## **Implementing The Attack**<br>
While we can do all the math by hand in Python, the server gives us a new N each time we connect. This means that we need to write a program to connect and calculate the signature for us, so I again opted to use a Python.

```python
from Crypto.Util.number import bytes_to_long,isPrime,inverse,long_to_bytes
# nc chall.nitdgplug.org 30093 
from pwn import * # pip install pwntools
import json
import base64
import codecs
r = remote('chall.nitdgplug.org',30093)
line = r.recvuntil(">").decode()
print(line)
r.sendline("P".encode())
r.recvline()
n = r.recvline()
n = n.decode().split(":")[-1][3:]
a = b'd4r3d3v!l'
M = bytes_to_long(a)
n = int(n,16)
print(n)
a = 2 #can choose any random number in range of n 
m1 = str(hex((a**65537)*M))[2:]
r.recvuntil("> ")
r.sendline("S".encode())
r.recvuntil(": ")
r.sendline(m1.encode())
r.recvline()
signature = r.recvline().decode().split(":")[-1][3:]
print(signature)
#print(long_to_bytes(pow(int(signature,16)//2,65537,n)))
M2s = (int(signature,16)//2)%n
M2s = str(hex(M2s))[2:]
r.recvuntil("> ")
r.sendline("V".encode())
print(r.recvuntil(": ").decode())
M = str(hex(M))[2:]
r.sendline(M.encode())
print(r.recvuntil(": ").decode())
r.sendline(M2s.encode())
print(r.recvuntil("}"))
#print(m1)
r.interactive()
``` 
**Flag:** GLUG{fl4g_15_53rv3d_xD_E9644V2GG0}<br>
<br>
`PS: This was my first time using pwntool library, so the above code could be optimized. I had a lot of fun trying to figure out how different functions work.Kudos to the Author for naming the challenge as Daredevil.`