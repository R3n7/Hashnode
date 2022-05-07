## Crypto0 & Crypto1 - Incognito 3.0

<h1>Crypto0</h1>
We are given a big base64 encoded ciphertext.we use cyberchef to decode the heaps of base64 text into plaintext. At a point it does not decode into meaningful text.

```
HQsdFSQcABUzNgc3CCw9HhUsPgsPWwA2LlQKZ0E=
```
This is what we get!
As the description said key.i assumed it was vigenere.but it was not.
Next, I tried Single Byte XOR. But no luck. Next i tried the Byte-XOR. as we know part of the plaintext as ictf{ .<br>
I Xored it with Ciphertext and got  'this_' . Something like this was on the description.
<br>as we guessed, it was the key.

`key:this_key_is_a_bit_annoying>_<`

```py
import base64

def byte_xor(ba1,ba2):
            return bytes([_a ^ _b for _a,_b in zip(ba1,ba2)])

key = 'ictf{'.encode()
key1 = b'this_key_is_a_bit_annoying>_<'
print(key)
print(key1)

end =  base64.b64decode('HQsdFSQcABUzNgc3CCw9HhUsPgsPWwA2LlQKZ0E=')

print(byte_xor(end,key1))
``` 
`Flag:ictf{well_this_was_ea4y_@348}`

 <h1>Crypto1</h1>
We can tell by looking at the ciphertext, it is Substitution.
we input the ciphertext to this site and the last line is the flag.
[Substitution Solver](https://www.guballa.de/substitution-solver)

`Flag:ictf{cngtultinsforfindingdis}`
