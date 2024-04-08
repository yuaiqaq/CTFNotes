### leak=e*d

### 1.推导过程

ed=$k\phi(n)$+1

ed=k*（n-（p+q）+1

$\frac{e*d-1}{n}=k+\frac{k*(1-(p+q))}{n} $

显然k ∗ ( 1 − ( p + q ) ) < n 

所以$\frac{k ∗ ( 1 − ( p + q ) )}{n}$  = − 1 

k = $\frac{e*d-1}{n}+1$ 

$\phi(n)$=$\frac{e*d-1}{n}/$$(\frac{e*d-1}{n}+1)$ 

### 2.封装脚本

~~~python
def fac_p_q_add_p_q(n,phi):
    p_q_sum = n - phi + 1
    difference_square = p_q_sum**2 - 4*n
    difference = gmpy2.iroot(difference_square,2)[0]
    p = (p_q_sum + difference) // 2
    q = p_q_sum - p
    if q < p:
        p,q = q,p
    return p,q
def leak_ed(ed,n)
	k=(e_d - 1) // n + 1
	phi=ed-1//k
    return fac_p_q_add_p_q(n,phi)
~~~



