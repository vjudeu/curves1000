Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value | 7fffffff fffffffe ffffe01d |
| b-value |                          7 |
| n-value | 7fffffff ffff75c7 82f41263 |
+---------+----------------------------+
| x-value | 33333333 33333332 ccccc00a |
| y-value | 34731b8d e9729c0d df8f05f5 |
+---------+----------------------------+
| (G/2).x |                          2 |
| (G/2).y |  2d71013 fb4a4b66 41e81341 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffeffffe01d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x3333333333333332ccccc00a,0x34731b8de9729c0ddf8f05f5)
h=1
E.set_order(0x7fffffffffff75c782f41263*h)
d=0x3fffffffffffbae3c17a0932
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffeffffe01d
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
