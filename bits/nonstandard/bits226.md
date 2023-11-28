Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |        3 ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff953 |
| b-value |                                                                       3 |
| n-value |        4 00000000 00000000 00000000 00016a93 b3e7b98c 30f13db0 92770785 |
+---------+-------------------------------------------------------------------------+
| x-value |        3 3fffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2ffffa92 |
| y-value |        2 8fffffff ffffffff ffffffff ffffffff ffffffff ffffffff 5bfffbb9 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffefffff953
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x33fffffffffffffffffffffffffffffffffffffffffffffff2ffffa92,0x28fffffffffffffffffffffffffffffffffffffffffffffff5bfffbb9)
h=1
E.set_order(0x400000000000000000000000000016a93b3e7b98c30f13db092770785*h)
d=0x20000000000000000000000000000b549d9f3dcc618789ed8493b83c3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffefffff953
modulo_root=(p+1)/4
x=0
b_value=3
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
