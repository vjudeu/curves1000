Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |    3ffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff740f |
| b-value |                                                                       5 |
| n-value |    40000 00000000 00000000 00000000 00287729 fe3ece24 6e3eeb44 b91f411f |
+---------+-------------------------------------------------------------------------+
| x-value |    347ff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2dff8d34 |
| y-value |    3139f 0554888d 46036fe1 34eb02aa 1e897c23 9b8ae829 ad92f403 5646fe95 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       3 |
| (G/2).y |    35379 4635853e c571ed24 98f6c01e bf2995a3 d4a96d1b a52c3318 40c3f2a0 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffeffff740f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x347ffffffffffffffffffffffffffffffffffffffffffffffffff2dff8d34,0x3139f0554888d46036fe134eb02aa1e897c239b8ae829ad92f4035646fe95)
h=1
E.set_order(0x4000000000000000000000000000000287729fe3ece246e3eeb44b91f411f*h)
d=0x2000000000000000000000000000000143b94ff1f6712371f75a25c8fa090
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffeffff740f
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
