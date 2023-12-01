Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |  7ffffff ffffffff ffffffff fffffffe ffffebd5 |
| b-value |                                            7 |
| n-value |  8000000 00000000 00001c73 d086a0dd e5e53f5b |
+---------+----------------------------------------------+
| x-value |  3333333 33333333 33333333 33333332 ccccc4ba |
| y-value |   a7518e 6183bf2d c7113f91 b4debb3a e8f1a5b6 |
+---------+----------------------------------------------+
| (G/2).x |                                            2 |
| (G/2).y |   c21672 23713e22 38ca1c1c 9b117afa 573c7a73 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffeffffebd5
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x3333333333333333333333333333332ccccc4ba,0xa7518e6183bf2dc7113f91b4debb3ae8f1a5b6)
h=1
E.set_order(0x80000000000000000001c73d086a0dde5e53f5b*h)
d=0x40000000000000000000e39e843506ef2f29fae
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffeffffebd5
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
