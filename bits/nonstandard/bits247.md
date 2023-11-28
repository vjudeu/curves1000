Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |   7fffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffda1d |
| b-value |                                                                       5 |
| n-value |   7fffff ffffffff ffffffff ffffffff f98c9b9b 942b99a8 451fe536 be3c4a59 |
+---------+-------------------------------------------------------------------------+
| x-value |   1d89d8 9d89d89d 89d89d89 d89d89d8 9d89d89d 89d89d89 d89d89d8 62761ea3 |
| y-value |   58e644 51b075e1 b57df4f1 ae79fbad 5f345730 b27dc443 15163fd9 87d03866 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |   3cb918 fcb152b9 11edf5d3 6c832ba1 c804ca34 e3b923aa 8461bc9d 051d4f80 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffffeffffda1d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1d89d89d89d89d89d89d89d89d89d89d89d89d89d89d89d89d89d862761ea3,0x58e64451b075e1b57df4f1ae79fbad5f345730b27dc44315163fd987d03866)
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
