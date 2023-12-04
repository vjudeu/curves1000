Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |    3ffff ffffffff ffffffff fffffffe ffffbd53 |
| b-value |                                            7 |
| n-value |    3ffff ffffffff fffffc00 12c5af71 d1cc7653 |
+---------+----------------------------------------------+
| x-value |    18568 9039b0ad 12073615 a240e6c2 52eddf6f |
| y-value |    3cc72 bf6eed15 ad127d12 fb9b3c97 83dd6336 |
+---------+----------------------------------------------+
| (G/2).x |                                            4 |
| (G/2).y |     e522 5f58fdfb c4a7b666 55c9f55c 2834df5d |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffeffffbd53
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x185689039b0ad12073615a240e6c252eddf6f,0x3cc72bf6eed15ad127d12fb9b3c9783dd6336)
h=1
E.set_order(0x3fffffffffffffffffc0012c5af71d1cc7653*h)
d=0x1fffffffffffffffffe000962d7b8e8e63b2a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffeffffbd53
modulo_root=(p+1)/4
x=0
b_value=7
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
