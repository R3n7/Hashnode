## John the Rocker - VishwaCTF 2022

Challenge Info:<br>

![2022-03-23 22_40_58-Window.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648055475026/DdaoFH0yy.png)
Description:None<br>
Files:`idrsa.id_rsa`<br>
Obviously we have to use john the ripper tool. So, i think we have crack the password to access the Private Key.Unless the jumbo version of John the Ripper is installed, we'll need to download ssh2john from GitHub since it's not included in the John the Ripper version that's installed in Kali Linux. (If you don't have John the Ripper installed, you can find out how to install it from its GitHub.)<br>
All we have to do is run it against the private key and direct the results to a new hash file using the ssh2john Python tool:
![2022-03-23 23_01_50-Window.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648056764184/q-GgsZGrZ.png)
We get hash with ssh2john.Now we want to find the password so use john.
<br>
![2022-03-23 23_02_09-Window.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648056840998/8jhP_17NF.png)
So we found the password. To view the password:
![2022-03-23 23_02_24-Window.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648056893211/AprM1o8D6.png)

Flag:`Vishwactf{!!**john**!!} `