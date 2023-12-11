Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |  1fffffe fffffda5 |
| b-value |                 7 |
| n-value |  1ffffff 2141e28d |
+---------+-------------------+
| x-value |  11a5a59 cd2d2be0 |
| y-value |   118ea8 a4c5be5a |
+---------+-------------------+
| (G/2).x |                 3 |
| (G/2).y |   dcc259 53a8a992 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffefffffda5
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x11a5a59cd2d2be0,0x118ea8a4c5be5a)
h=1
E.set_order(0x1ffffff2141e28d*h)
d=0xffffff90a0f147
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffefffffda5
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