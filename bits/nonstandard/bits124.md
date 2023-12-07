Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |  fffffff ffffffff fffffffe ffffd48d |
| b-value |                                   7 |
| n-value | 10000000 00000000 17793caf 6fe2ed85 |
+---------+-------------------------------------+
| x-value |  8d2d2d2 d2d2d2d2 d2d2d2d2 45a58daf |
| y-value |  8111fce 74403ec0 b9e7549d f71a7dcb |
+---------+-------------------------------------+
| (G/2).x |                                   3 |
| (G/2).y |  3840c7f bad3d7a4 1c571aba 7e0491e2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffeffffd48d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x8d2d2d2d2d2d2d2d2d2d2d245a58daf,0x8111fce74403ec0b9e7549df71a7dcb)
h=1
E.set_order(0x100000000000000017793caf6fe2ed85*h)
d=0x8000000000000000bbc9e57b7f176c3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffeffffd48d
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
