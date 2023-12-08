Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |    3ffff ffffffff fffffffe ffffc815 |
| b-value |                                   5 |
| n-value |    3ffff ffffffff fc161c9c 39a6622f |
+---------+-------------------------------------+
| x-value |     ec4e c4ec4ec4 ec4ec4ec 13b12e2b |
| y-value |    34772 bb1214f4 bc404e7f 8d8824d4 |
+---------+-------------------------------------+
| (G/2).x |                                   2 |
| (G/2).y |    1917a 927e4358 1986059f 35c54b76 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffeffffc815
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xec4ec4ec4ec4ec4ec4ec13b12e2b,0x34772bb1214f4bc404e7f8d8824d4)
h=1
E.set_order(0x3fffffffffffffc161c9c39a6622f*h)
d=0x1fffffffffffffe0b0e4e1cd33118
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffeffffc815
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=0
b_value=5
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    first_y=y_square.powermod(first_modulo_root,p)
    is_on_curve=(first_y.powermod(2,p)==y_square)
    y=first_y
    if not is_on_curve:
        second_y=(first_y*second_multiplier)%p
        is_on_curve=(second_y.powermod(2,p)==y_square)
        y=second_y
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
