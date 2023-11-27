Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |    fffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff7403 |
| b-value |                                                                       3 |
| n-value |   100000 00000000 00000000 00000000 02053aa8 09c6c784 e63c49a1 c07986e5 |
+---------+-------------------------------------------------------------------------+
| x-value |    cffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2fff8e41 |
| y-value |    63fff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 9bffc951 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffffeffff7403
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xcffffffffffffffffffffffffffffffffffffffffffffffffffff2fff8e41,0x63fffffffffffffffffffffffffffffffffffffffffffffffffff9bffc951)
h=1
E.set_order(0x10000000000000000000000000000002053aa809c6c784e63c49a1c07986e5*h)
d=0x8000000000000000000000000000001029d5404e363c2731e24d0e03cc373
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffffeffff7403
modulo_root=(p+1)/4
x=0
b_value=3
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
