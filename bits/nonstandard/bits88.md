Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |   ffffff fffffffe fffff01d |
| b-value |                          7 |
| n-value |   ffffff ffffe9ee ec45b453 |
+---------+----------------------------+
| x-value |   2edf8c 9ea5dbf1 64f52bf7 |
| y-value |   2eb9d1 387b4416 2c91efdf |
+---------+----------------------------+
| (G/2).x |                          4 |
| (G/2).y |    461d7 0f8eae4a 166c03da |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffefffff01d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x2edf8c9ea5dbf164f52bf7,0x2eb9d1387b44162c91efdf)
h=1
E.set_order(0xffffffffffe9eeec45b453*h)
d=0x7ffffffffff4f77622da2a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffefffff01d
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
