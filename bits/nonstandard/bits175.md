Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |     7fff ffffffff ffffffff ffffffff fffffffe ffff717d |
| b-value |                                                     7 |
| n-value |     8000 00000000 00000000 00d7fdee ebab303c 0b722263 |
+---------+-------------------------------------------------------+
| x-value |     3333 33333333 33333333 33333333 33333332 cccc93ca |
| y-value |     1734 236ba61d 179a5478 b562e570 7e0609dd c7ee85f5 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |     2919 67c61c70 aa01a8b5 3f269516 efdf2db2 5206e050 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffeffff717d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x333333333333333333333333333333333332cccc93ca,0x1734236ba61d179a5478b562e5707e0609ddc7ee85f5)
h=1
E.set_order(0x8000000000000000000000d7fdeeebab303c0b722263*h)
d=0x40000000000000000000006bfef775d5981e05b91132
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffeffff717d
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=0
b_value=7
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
