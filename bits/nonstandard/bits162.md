Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |        3 ffffffff ffffffff ffffffff fffffffe ffffe375 |
| b-value |                                                     7 |
| n-value |        4 00000000 00000000 000367d8 6025871c 946bf237 |
+---------+-------------------------------------------------------+
| x-value |        2 66666666 66666666 66666666 66666665 ccccbbab |
| y-value |          dd4a7a86 aae4c8b0 1875eee5 9aaccbbd ccabd5e4 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |        1 f6ef1677 e17c53be c33a934f a55a2c23 b9e3a04c |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffeffffe375
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x266666666666666666666666666666665ccccbbab,0xdd4a7a86aae4c8b01875eee59aaccbbdccabd5e4)
h=1
E.set_order(0x40000000000000000000367d86025871c946bf237*h)
d=0x200000000000000000001b3ec3012c38e4a35f91c
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffeffffe375
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
