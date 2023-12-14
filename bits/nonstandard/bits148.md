Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |    fffff ffffffff ffffffff fffffffe fffffc4d |
| b-value |                                            5 |
| n-value |    fffff ffffffff fffffb18 53996d57 6439fcaf |
+---------+----------------------------------------------+
| x-value |    2ffff ffffffff ffffffff ffffffff cfffff51 |
| y-value |    5bfff ffffffff ffffffff ffffffff a3fffea7 |
+---------+----------------------------------------------+
| (G/2).x |    fffff ffffffff ffffffff fffffffe fffffc4c |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffefffffc4d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x2ffffffffffffffffffffffffffffcfffff51,0x5bfffffffffffffffffffffffffffa3fffea7)
h=1
E.set_order(0xffffffffffffffffffb1853996d576439fcaf*h)
d=0x7fffffffffffffffffd8c29ccb6abb21cfe58
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffefffffc4d
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
