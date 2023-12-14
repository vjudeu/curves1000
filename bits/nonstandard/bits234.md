Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |      3ff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff8c15 |
| b-value |                                                                       5 |
| n-value |      3ff ffffffff ffffffff ffffffff ffc4b5e5 3d714de7 3b82bbe4 a4125039 |
+---------+-------------------------------------------------------------------------+
| x-value |      2bf ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 4fffb051 |
| y-value |      3ef ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 03ff8de0 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |      3ff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff8c14 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffeffff8c15
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x2bfffffffffffffffffffffffffffffffffffffffffffffffff4fffb051,0x3efffffffffffffffffffffffffffffffffffffffffffffffff03ff8de0)
h=1
E.set_order(0x3ffffffffffffffffffffffffffffc4b5e53d714de73b82bbe4a4125039*h)
d=0x1ffffffffffffffffffffffffffffe25af29eb8a6f39dc15df25209281d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffeffff8c15
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
