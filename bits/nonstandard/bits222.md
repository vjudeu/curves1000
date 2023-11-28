Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdacf |
| b-value |                                                              5 |
| n-value | 3fffffff ffffffff ffffffff ffff1676 af8f9385 db82f0e7 e58c3aa1 |
+---------+----------------------------------------------------------------+
| x-value | 147fffff ffffffff ffffffff ffffffff ffffffff ffffffff adfff416 |
| y-value |  c7429e5 690d31ad 7423313c 515eea56 35fbae54 6d5f7286 63df62f1 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              3 |
| (G/2).y | 167033be c30134b2 6ca5b5ea 21c35f6f ecedfc8e 347a9820 361061c6 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffeffffdacf
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x147fffffffffffffffffffffffffffffffffffffffffffffadfff416,0xc7429e5690d31ad7423313c515eea5635fbae546d5f728663df62f1)
h=1
E.set_order(0x3fffffffffffffffffffffffffff1676af8f9385db82f0e7e58c3aa1*h)
d=0x1fffffffffffffffffffffffffff8b3b57c7c9c2edc17873f2c61d51
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffeffffdacf
modulo_root=(p+1)/4
x=0
b_value=5
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
