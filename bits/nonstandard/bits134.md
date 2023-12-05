Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |       3f ffffffff ffffffff fffffffe ffffeefd |
| b-value |                                            5 |
| n-value |       3f ffffffff fffffffc ce1b7f93 d3a202c7 |
+---------+----------------------------------------------+
| x-value |        2 58469ee5 8469ee58 469ee584 608d3d2d |
| y-value |       32 111ed35b edde2d25 7855dc68 3c4a2432 |
+---------+----------------------------------------------+
| (G/2).x |                                            7 |
| (G/2).y |        5 cf30547c 52488044 344e4aa5 8e202fc6 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffeffffeefd
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x258469ee58469ee58469ee584608d3d2d,0x32111ed35bedde2d257855dc683c4a2432)
h=1
E.set_order(0x3ffffffffffffffffcce1b7f93d3a202c7*h)
d=0x1ffffffffffffffffe670dbfc9e9d10164
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffeffffeefd
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
