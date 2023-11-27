Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    3ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffd8b3 |
| b-value |                                                              3 |
| n-value |    3ffff ffffffff ffffffff fffffc18 f5b7eac1 c662bb78 b3924b51 |
+---------+----------------------------------------------------------------+
| x-value |    33fff ffffffff ffffffff ffffffff ffffffff ffffffff 2fffe010 |
| y-value |     8fff ffffffff ffffffff ffffffff ffffffff ffffffff dbfffa79 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffeffffd8b3
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x33fffffffffffffffffffffffffffffffffffffffffff2fffe010,0x8fffffffffffffffffffffffffffffffffffffffffffdbfffa79)
h=1
E.set_order(0x3fffffffffffffffffffffffffc18f5b7eac1c662bb78b3924b51*h)
d=0x1fffffffffffffffffffffffffe0c7adbf560e3315dbc59c925a9
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffeffffd8b3
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
