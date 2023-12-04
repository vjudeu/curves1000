Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |    fffff ffffffff ffffffff fffffffe fffffc4d |
| b-value |                                            5 |
| n-value |    fffff ffffffff fffffb18 53996d57 6439fcaf |
+---------+----------------------------------------------+
| x-value |    b13b1 3b13b13b 13b13b13 b13b13b0 89d89af9 |
| y-value |    f5038 b73f3c57 5b3e39cd 4274b4bc e15b7f51 |
+---------+----------------------------------------------+
| (G/2).x |                                            2 |
| (G/2).y |    746d0 02ff3702 10254142 52e14b00 8a16da7f |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffefffffc4d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xb13b13b13b13b13b13b13b13b13b089d89af9,0xf5038b73f3c575b3e39cd4274b4bce15b7f51)
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
