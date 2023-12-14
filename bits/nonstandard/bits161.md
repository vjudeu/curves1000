Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |        1 ffffffff ffffffff ffffffff fffffffe ffffe46d |
| b-value |                                                     5 |
| n-value |        2 00000000 00000000 0000947d 8da64c6f 81ae8bcb |
+---------+-------------------------------------------------------+
| x-value |          5fffffff ffffffff ffffffff ffffffff cffffad7 |
| y-value |        1 b7ffffff ffffffff ffffffff ffffffff 23ffe849 |
+---------+-------------------------------------------------------+
| (G/2).x |         1ffffffff ffffffff ffffffff fffffffe ffffe46c |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffeffffe46d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x5fffffffffffffffffffffffffffffffcffffad7,0x1b7ffffffffffffffffffffffffffffff23ffe849)
h=1
E.set_order(0x200000000000000000000947d8da64c6f81ae8bcb*h)
d=0x1000000000000000000004a3ec6d32637c0d745e6
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffeffffe46d
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
