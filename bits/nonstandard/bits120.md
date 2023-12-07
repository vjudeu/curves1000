Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |   ffffff ffffffff fffffffe fffff2ed |
| b-value |                                   5 |
| n-value |   ffffff ffffffff e014de50 f7d84bff |
+---------+-------------------------------------+
| x-value |   c4ec4e c4ec4ec4 ec4ec4eb 89d8937a |
| y-value |   2709c8 39cc9de7 cf86b53e b82df5b4 |
+---------+-------------------------------------+
| (G/2).x |                                   2 |
| (G/2).y |   55a7f7 4d97605c a0627a4b 3f7f81da |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffefffff2ed
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xc4ec4ec4ec4ec4ec4ec4eb89d8937a,0x2709c839cc9de7cf86b53eb82df5b4)
h=1
E.set_order(0xffffffffffffffe014de50f7d84bff*h)
d=0x7ffffffffffffff00a6f287bec2600
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffefffff2ed
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
