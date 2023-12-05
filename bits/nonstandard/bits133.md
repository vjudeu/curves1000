Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |       1f ffffffff ffffffff fffffffe ffffee45 |
| b-value |                                            5 |
| n-value |       1f ffffffff fffffff4 cd58e502 117a74e1 |
+---------+----------------------------------------------+
| x-value |        9 d89d89d8 9d89d89d 89d89d89 89d89814 |
| y-value |       1f a053f747 482258ab 72208be8 68d010f1 |
+---------+----------------------------------------------+
| (G/2).x |                                            2 |
| (G/2).y |        8 7df81340 f27d73c5 f4b77968 d87fad6c |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffeffffee45
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x9d89d89d89d89d89d89d89d8989d89814,0x1fa053f747482258ab72208be868d010f1)
h=1
E.set_order(0x1ffffffffffffffff4cd58e502117a74e1*h)
d=0xffffffffffffffffa66ac728108bd3a71
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffeffffee45
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
