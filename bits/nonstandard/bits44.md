Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |      ffe fffffcdd |
| b-value |                 5 |
| n-value |      ffe ffb55869 |
+---------+-------------------+
| x-value |      13a ffffffc1 |
| y-value |      cad e2af3983 |
+---------+-------------------+
| (G/2).x |                 2 |
| (G/2).y |      71f 5e86b98a |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0xffefffffcdd
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x13affffffc1,0xcade2af3983)
h=1
E.set_order(0xffeffb55869*h)
d=0x7ff7fdaac35
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffefffffcdd
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
