Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value | 7fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffef53 |
| b-value |                                                                       5 |
| n-value | 7fffffff ffffffff ffffffff ffffffff 00f26097 ca79ff9a bcdac70f f2f55f6d |
+---------+-------------------------------------------------------------------------+
| x-value | 6fffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 1ffff167 |
| y-value | 2b5f8ca9 034a06ef 4d6c422f d53b29f7 42c3005b 7807ca02 7e13d258 2d276ef1 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y | 3c014f88 b09d0319 4d50b3d1 f0c84019 6c545c0d 2e74fa27 4b693efe 08475f30 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffffffeffffef53
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x6fffffffffffffffffffffffffffffffffffffffffffffffffffffff1ffff167,0x2b5f8ca9034a06ef4d6c422fd53b29f742c3005b7807ca027e13d2582d276ef1)
h=1
E.set_order(0x7fffffffffffffffffffffffffffffff00f26097ca79ff9abcdac70ff2f55f6d*h)
d=0x3fffffffffffffffffffffffffffffff8079304be53cffcd5e6d6387f97aafb7
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffffffeffffef53
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
