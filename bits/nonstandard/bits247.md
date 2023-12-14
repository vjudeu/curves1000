Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |   7fffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffda1d |
| b-value |                                                                       5 |
| n-value |   7fffff ffffffff ffffffff ffffffff f98c9b9b 942b99a8 451fe536 be3c4a59 |
+---------+-------------------------------------------------------------------------+
| x-value |   17ffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff cffff8e8 |
| y-value |   4dffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 63ffe8e5 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |   7fffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffda1c |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffffeffffda1d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x17ffffffffffffffffffffffffffffffffffffffffffffffffffffcffff8e8,0x4dffffffffffffffffffffffffffffffffffffffffffffffffffff63ffe8e5)
h=1
E.set_order(0x7ffffffffffffffffffffffffffffff98c9b9b942b99a8451fe536be3c4a59*h)
d=0x3ffffffffffffffffffffffffffffffcc64dcdca15ccd4228ff29b5f1e252d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffffeffffda1d
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
