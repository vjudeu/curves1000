Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |        1 ffffffff ffffffff ffffffff fffffffe ffffe46d |
| b-value |                                                     5 |
| n-value |        2 00000000 00000000 0000947d 8da64c6f 81ae8bcb |
+---------+-------------------------------------------------------+
| x-value |        1 d89d89d8 9d89d89d 89d89d89 d89d89d7 b13afa3c |
| y-value |          917bd4bf 221a29ad 9378bdf3 ab0d4a84 edc420ab |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |          18cddf72 ab385b07 faec985c 186e56f6 8483105d |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffeffffe46d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1d89d89d89d89d89d89d89d89d89d89d7b13afa3c,0x917bd4bf221a29ad9378bdf3ab0d4a84edc420ab)
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
