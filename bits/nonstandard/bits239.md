Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |     7fff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffeef3 |
| b-value |                                                                       3 |
| n-value |     7fff ffffffff ffffffff ffffffff fea1b9f6 d672116a bac7fbda 95f9dea9 |
+---------+-------------------------------------------------------------------------+
| x-value |     67ff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2ffff224 |
| y-value |     6dff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 23fff159 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |     7fff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffeef1 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffeffffeef3
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x67ffffffffffffffffffffffffffffffffffffffffffffffffff2ffff224,0x6dffffffffffffffffffffffffffffffffffffffffffffffffff23fff159)
h=1
E.set_order(0x7ffffffffffffffffffffffffffffea1b9f6d672116abac7fbda95f9dea9*h)
d=0x3fffffffffffffffffffffffffffff50dcfb6b3908b55d63fded4afcef55
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffeffffeef3
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
