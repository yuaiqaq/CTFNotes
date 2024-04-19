# robin密码

## 密码格式：

~~~
RSA的变式
随机选择两个大宿主p、q，满足条件为p≡q≡3 mod 4，即p和q可用4k+3的形式表示，n=p×q。
公钥是n，私钥是p、q。同时e=2
~~~

## 加密与解密

加密：c$\equiv$m^2^ mod n 

解密：r^2^$\equiv$m^2^$\equiv$c mod p

​	    s^2^$\equiv$m^2^$\equiv$ c mod q

​	   m $\equiv$ r mod  p 

​	   m $\equiv$ s mod q

其中:   r$\equiv$ $\pm c^\frac{p+1}{4}$ mod p

​	    s$\equiv$ $\pm c^{\frac{p+1}{4}}$ mod q

依次带入r，s就可以得到四个解，m为四个解的一个

证明过程：

[Rabin加密原理 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/533927542)

## 例题 buuctf 坏蛋是雷宾

题目描述：

老牌刺客之王混进了女王的住所。一天，女王得到了一个匿名举报，说她的侍卫里有一个刺客，叫做Rabin，而他的信息就在一份文件里，文件中有附带一个Pk，是523798549，密文是162853095，校验码二进制值是110001，根据说明是放在明文后一起加密的，明文与密文长度相同。加密算法和这位老牌刺客同名。快拯救女王，答案是求得的明文，进行32位md5小写哈希字符串，提交即可。 注意：得到的 flag 请包上 flag{} 提交



题解：

 ~~~ 
 根据题目：m=162853095,e=2,n=523798549
 n很小因此可以直接分解出p,q
 根据解密公式解密出r，s得到再计算出明文
 将四个可能的明文转化成二进制，比较最后的校验位
 最后进行用md5小写哈希运算得到flag
 ~~~

解题脚本：

~~~python
from Factors import *
from gmpy2 import *
import hashlib
def robin(p,q,c):
    n=p*q
    e=2
    inv_p = gmpy2.invert(p, q)
    inv_q = gmpy2.invert(q, p)


    mp = pow(c, int((p + 1) / 4), p)
    mq = pow(c, int((q + 1) / 4), q)


    a = (inv_p * p * mq + inv_q * q * mp) % n
    b = n - int(a)
    c = (inv_p * p * mq - inv_q * q * mp) % n
    d = n - int(c)

    for i in (a, b, c, d):
        print(bin(i))
    return a,b,c,d
robin(10663, 49123, 162853095)

#在得出的四个解中，找到二进制符合题目的解
# 0b10010011100100100101010110001
# 0b1100110001100011110101100100
# 0b110111001100000100101111001
# 0b11000010100100111111010011100
m='10010011100100100101010'
mc=str(int(m,2))
md=hashlib.md5()
md.update(mc.encode("utf8"))
flag = md.hexdigest()
print("flag{"+str(flag)+'}')
#flag{ca5cec442b2734735406d78c88e90f35}


~~~

