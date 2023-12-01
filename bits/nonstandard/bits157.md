Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value | 1fffffff ffffffff ffffffff fffffffe ffffea6d |
| b-value |                                            7 |
| n-value | 20000000 00000000 000096d5 c62ae88c 2a1a6597 |
+---------+----------------------------------------------+
| x-value |  a5dbf19 3d4bb7e3 27a976fc 64f52edf 39b0a615 |
| y-value | 118827cc 53af17dd c44559d4 5ceb3595 a0893a54 |
+---------+----------------------------------------------+
| (G/2).x |                                            4 |
| (G/2).y |  ebb9bae e54b0294 c3a7a3cc b0df1b5c 3abbbc39 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffeffffea6d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xa5dbf193d4bb7e327a976fc64f52edf39b0a615,0x118827cc53af17ddc44559d45ceb3595a0893a54)
h=1
E.set_order(0x2000000000000000000096d5c62ae88c2a1a6597*h)
d=0x100000000000000000004b6ae3157446150d32cc
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffeffffea6d
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
