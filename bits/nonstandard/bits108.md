Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |      fff ffffffff fffffffe ffffeb0d |
| b-value |                                   7 |
| n-value |      fff ffffffff ff8227d8 d8338acf |
+---------+-------------------------------------+
| x-value |      32d 2d2d2d2d 2d2d2d2c fa5a5631 |
| y-value |      7e2 993d81db d2773d45 55fd6b01 |
+---------+-------------------------------------+
| (G/2).x |                                   3 |
| (G/2).y |      34b 4b2a37c9 1940143c e41adb52 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffeffffeb0d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x32d2d2d2d2d2d2d2d2cfa5a5631,0x7e2993d81dbd2773d4555fd6b01)
h=1
E.set_order(0xfffffffffffff8227d8d8338acf*h)
d=0x7ffffffffffffc113ec6c19c568
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffeffffeb0d
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
