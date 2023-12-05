Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |       7f ffffffff ffffffff fffffffe ffffabe5 |
| b-value |                                            7 |
| n-value |       7f ffffffff ffffffea c2f424cd 5a4499e1 |
+---------+----------------------------------------------+
| x-value |       4c cccccccc cccccccc cccccccc 333300bb |
| y-value |       7c 59cbdb89 ede2a9ed 5ea60b75 73df6463 |
+---------+----------------------------------------------+
| (G/2).x |                                            2 |
| (G/2).y |        3 57b80a39 79bd53a7 1cedbcf5 d8cde179 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffeffffabe5
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x4ccccccccccccccccccccccccc333300bb,0x7c59cbdb89ede2a9ed5ea60b7573df6463)
h=1
E.set_order(0x7fffffffffffffffeac2f424cd5a4499e1*h)
d=0x3ffffffffffffffff5617a1266ad224cf1
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffeffffabe5
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
