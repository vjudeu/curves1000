Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    1ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdced |
| b-value |                                                              5 |
| n-value |    20000 00000000 00000000 00000246 f6bf684a 97ecd7a7 7b2d5e75 |
+---------+----------------------------------------------------------------+
| x-value |     5fff ffffffff ffffffff ffffffff ffffffff ffffffff cffff96f |
| y-value |    1b7ff ffffffff ffffffff ffffffff ffffffff ffffffff 23ffe1d7 |
+---------+----------------------------------------------------------------+
| (G/2).x |    1ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdcec |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffeffffdced
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x5fffffffffffffffffffffffffffffffffffffffffffcffff96f,0x1b7ffffffffffffffffffffffffffffffffffffffffff23ffe1d7)
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
x=p
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
        x-=1
```
