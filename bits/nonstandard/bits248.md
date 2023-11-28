Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |   ffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffbf3f |
| b-value |                                                                       3 |
| n-value |   ffffff ffffffff ffffffff ffffffff e03c7b2e ae8b341f 91676373 76af61af |
+---------+-------------------------------------------------------------------------+
| x-value |   8fffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6fffdb92 |
| y-value |   d3ffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2bffca60 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffffeffffbf3f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x8fffffffffffffffffffffffffffffffffffffffffffffffffffff6fffdb92,0xd3ffffffffffffffffffffffffffffffffffffffffffffffffffff2bffca60)
h=1
E.set_order(0xffffffffffffffffffffffffffffffe03c7b2eae8b341f9167637376af61af*h)
d=0x7ffffffffffffffffffffffffffffff01e3d9757459a0fc8b3b1b9bb57b0d8
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffffeffffbf3f
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
