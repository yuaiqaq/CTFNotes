# 键盘加密

哇，这里有压缩包的密码哦，于是我低下了头，看向了我的双手，试图从中找到某些规律
xdfv ujko98 edft54 xdfv pok,.; wsdr43

看键盘，键盘包住的是circle

# AES

题目来源 buuctf [ACTF新生赛2020]crypto-aes

AES加密 [AES加密算法原理的详细介绍与实现-CSDN博客](https://blog.csdn.net/qq_28205153/article/details/55798628)

~~~python
import libnum
from Crypto.Cipher import AES
from Crypto.Util.number import bytes_to_long, long_to_bytes
import os
key = os.urandom(2) * 16    #两个字符随机循环32B高16位不变
iv = os.urandom(16)         #随机16个字符
xor=91144196586662942563895769614300232343026691029427747065707381728622849079757
enc_flag=b'\x8c-\xcd\xde\xa7\xe9\x7f.b\x8aKs\xf1\xba\xc75\xc4d\x13\x07\xac\xa4&\xd6\x91\xfe\xf3\x14\x10|\xf8p'
out=long_to_bytes(xor)
print(out)
key=out[:16]*2
print(key)
iv = bytes_to_long(key[16:]) ^ bytes_to_long(out[16:])  #低16位异或得到原来的iv
iv = long_to_bytes(iv)
aes = AES.new(key, AES.MODE_CBC, iv)
flag = aes.decrypt(enc_flag)
print(flag)
~~~

