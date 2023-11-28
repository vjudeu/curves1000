Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |     3fff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffe0f |
| b-value |                                                                       3 |
| n-value |     4000 00000000 00000000 00000000 00a5630b 59b6a049 0c0e7f38 c578229b |
+---------+-------------------------------------------------------------------------+
| x-value |     23ff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6ffffee7 |
| y-value |      4ff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ebffffd9 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffefffffe0f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23ffffffffffffffffffffffffffffffffffffffffffffffffff6ffffee7,0x4ffffffffffffffffffffffffffffffffffffffffffffffffffebffffd9)
h=1
E.set_order(0x400000000000000000000000000000a5630b59b6a0490c0e7f38c578229b*h)
d=0x20000000000000000000000000000052b185acdb502486073f9c62bc114e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffefffffe0f
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
