Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff ffffffff fffffffe ffffdd15 |
| b-value |                                                     7 |
| n-value | 40000000 00000000 00000000 b70af987 4eb781e1 2b4bda23 |
+---------+-------------------------------------------------------+
| x-value | 19999999 99999999 99999999 99999999 99999999 3333253a |
| y-value | 2f1edbf9 9efef412 47732769 9fc4d782 06a37975 fee80aa7 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |  82edcab af2c2ab5 2e05b6aa 3c90d284 9ab959df 2c0f93fe |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffeffffdd15
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x19999999999999999999999999999999999999993333253a,0x2f1edbf99efef412477327699fc4d78206a37975fee80aa7)
h=1
E.set_order(0x400000000000000000000000b70af9874eb781e12b4bda23*h)
d=0x2000000000000000000000005b857cc3a75bc0f095a5ed12
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffeffffdd15
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
