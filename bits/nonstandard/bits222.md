Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdacf |
| b-value |                                                              5 |
| n-value | 3fffffff ffffffff ffffffff ffff1676 af8f9385 db82f0e7 e58c3aa1 |
+---------+----------------------------------------------------------------+
| x-value | 147fffff ffffffff ffffffff ffffffff ffffffff ffffffff adfff416 |
| y-value | 338bd61a 96f2ce52 8bdccec3 aea115a9 ca0451ab 92a08d78 9c2077de |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              3 |
| (G/2).y | 298fcc41 3cfecb4d 935a4a15 de3ca090 13120371 cb8567de c9ef7909 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffeffffdacf
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x147fffffffffffffffffffffffffffffffffffffffffffffadfff416,0x338bd61a96f2ce528bdccec3aea115a9ca0451ab92a08d789c2077de)
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
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
