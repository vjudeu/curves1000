Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |        7 ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffb5ab |
| b-value |                                                                       3 |
| n-value |        7 ffffffff ffffffff ffffffff fffc4efb c36939ff 371e4e2d a561be4f |
+---------+-------------------------------------------------------------------------+
| x-value |        2 7fffffff ffffffff ffffffff ffffffff ffffffff ffffffff afffe8c4 |
| y-value |        4 1fffffff ffffffff ffffffff ffffffff ffffffff ffffffff 7bffd9ac |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffeffffb5ab
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x27fffffffffffffffffffffffffffffffffffffffffffffffafffe8c4,0x41fffffffffffffffffffffffffffffffffffffffffffffff7bffd9ac)
h=1
E.set_order(0x7fffffffffffffffffffffffffffc4efbc36939ff371e4e2da561be4f*h)
d=0x3fffffffffffffffffffffffffffe277de1b49cff9b8f2716d2b0df28
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffeffffb5ab
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
