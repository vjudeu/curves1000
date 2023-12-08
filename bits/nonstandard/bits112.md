Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |     ffff ffffffff fffffffe ffffeddd |
| b-value |                                   5 |
| n-value |     ffff ffffffff ff20fcfc 86a3f81b |
+---------+-------------------------------------+
| x-value |     de9b d37a6f4d e9bd37a6 1642b894 |
| y-value |     9c26 9e992748 7afc3ec8 60dde285 |
+---------+-------------------------------------+
| (G/2).x |                                   4 |
| (G/2).y |     6908 9978cc65 a327d0fc 96ace4c3 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffeffffeddd
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xde9bd37a6f4de9bd37a61642b894,0x9c269e9927487afc3ec860dde285)
h=1
E.set_order(0xffffffffffffff20fcfc86a3f81b*h)
d=0x7fffffffffffff907e7e4351fc0e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffeffffeddd
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
