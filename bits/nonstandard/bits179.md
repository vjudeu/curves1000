Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |    7ffff ffffffff ffffffff ffffffff fffffffe ffffc51d |
| b-value |                                                     7 |
| n-value |    80000 00000000 00000000 01dfdf72 df233920 fca32fbd |
+---------+-------------------------------------------------------+
| x-value |     a5a5 a5a5a5a5 a5a5a5a5 a5a5a5a5 a5a5a5a5 90f0ec2d |
| y-value |    24da3 b2645b3d f187717e 05a9a969 b6a1f84a 957d6714 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     3 |
| (G/2).y |    18ec0 3c773986 5afc62fc 9d3ab375 2499a8eb 253468b3 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffeffffc51d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xa5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a590f0ec2d,0x24da3b2645b3df187717e05a9a969b6a1f84a957d6714)
h=1
E.set_order(0x80000000000000000000001dfdf72df233920fca32fbd*h)
d=0x40000000000000000000000efefb96f919c907e5197df
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffeffffc51d
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
