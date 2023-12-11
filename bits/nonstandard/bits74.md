Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |      3ff fffffffe fffffc1d |
| b-value |                          7 |
| n-value |      3ff ffffffcb 6889f08b |
+---------+----------------------------+
| x-value |       52 d2d2d2d2 be1e1dcd |
| y-value |       21 5f0b8fd0 13bee75e |
+---------+----------------------------+
| (G/2).x |                          3 |
| (G/2).y |      116 6b2b71ae 64910742 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffefffffc1d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x52d2d2d2d2be1e1dcd,0x215f0b8fd013bee75e)
h=1
E.set_order(0x3ffffffffcb6889f08b*h)
d=0x1ffffffffe5b444f846
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffefffffc1d
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
