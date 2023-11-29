Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    1ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdced |
| b-value |                                                              5 |
| n-value |    20000 00000000 00000000 00000246 f6bf684a 97ecd7a7 7b2d5e75 |
+---------+----------------------------------------------------------------+
| x-value |     7627 62762762 76276276 27627627 62762762 76276275 ec4ebcd3 |
| y-value |    1eb0d 83ca449f 2869e8c8 1ffe3c44 9e09a22a 20e061ac e8b1c10b |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              2 |
| (G/2).y |     4462 75361059 a8a64c19 068a495d 7323096b 5eb3486e 396a2ea8 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffeffffdced
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x76276276276276276276276276276276276276276275ec4ebcd3,0x1eb0d83ca449f2869e8c81ffe3c449e09a22a20e061ace8b1c10b)
h=1
E.set_order(0x20000000000000000000000000246f6bf684a97ecd7a77b2d5e75*h)
d=0x100000000000000000000000001237b5fb4254bf66bd3bd96af3b
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffeffffdced
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
