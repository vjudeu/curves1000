Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |        7 ffffffff ffffffff ffffffff ffffffff fffffffe ffffb893 |
| b-value |                                                              3 |
| n-value |        8 00000000 00000000 00000005 9c9afebc 1969c360 f23a9e0d |
+---------+----------------------------------------------------------------+
| x-value |        6 7fffffff ffffffff ffffffff ffffffff ffffffff 2fffc5f6 |
| y-value |        5 1fffffff ffffffff ffffffff ffffffff ffffffff 5bffd23e |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffeffffb893
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x67fffffffffffffffffffffffffffffffffffffff2fffc5f6,0x51fffffffffffffffffffffffffffffffffffffff5bffd23e)
h=1
E.set_order(0x80000000000000000000000059c9afebc1969c360f23a9e0d*h)
d=0x4000000000000000000000002ce4d7f5e0cb4e1b0791d4f07
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffeffffb893
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
