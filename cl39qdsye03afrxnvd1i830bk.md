## Easy Crypto Challenge - Space Heroes CTF 2022

Challenge description:

My slingshotter cousin Maneo just discovered a new cryptography scheme and has been raving about it since. I was trying to tell him the importance of setting a large, random private key, but he wouldn't listen. Guess security isn't as important as how many thousands of credits he can win in his next race around the system.

I've recovered a message sent to him detailing the finish line. Can you decrypt the message to find the coordinates so I can beat him there? I'll give you a percentage of the score!

Enter flag as shctf{x_y}, where x & y are the coordinates of the decrypted point.


``````
y^2 = x^3 + ax + b
a = 3820149076078175358
b = 1296618846080155687
modulus = 11648516937377897327
G = (4612592634107804164, 6359529245154327104)
PubKey = (9140537108692473465, 10130615023776320406)
k*G = (7657281011886994152, 10408646581210897023)
C = (5414448462522866853, 5822639685215517063)


What's the message?
``````
This Challenge is based on ECDH as the description states the private key is small.<br>
So i tried to find that and found a interesting writeup on how to do that.<br>
[https://ctftime.org/writeup/29157](https://ctftime.org/writeup/29157 "https://ctftime.org/writeup/29157")

![Screenshot 2022-05-17 104014.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652764237054/rTp1MNflY.png align="left")
Now we have that private key. we can find the shared key, <br>
`private key x k*G` <br>
As we know that both C,
Shared key are points in the eliptic curve.we can say that the encryption was addition of points.<br> 
Now, we just need to subtract C - S to get the Points.  

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1652764165877/MqtL1EnqJ.png align="left")
Flag:
`shctf{8042846929834025144_1238981380437369357}`
