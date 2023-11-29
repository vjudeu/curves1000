Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |     3fff ffffffff ffffffff ffffffff fffffffe fffff155 |
| b-value |                                                     5 |
| n-value |     3fff ffffffff ffffffff ff015f16 4053415f 93ca0bdd |
+---------+-------------------------------------------------------+
| x-value |      5ca b705cab7 05cab705 cab705ca b705cab6 ee9fd99c |
| y-value |     1506 32dba40a d00cc4c5 b37a33f1 b41e630d f3e00d52 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     6 |
| (G/2).y |      81c ff74eab8 1339639d 3f8b67bc 3e9f5a61 2eb2935c |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffefffff155
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x5cab705cab705cab705cab705cab705cab6ee9fd99c,0x150632dba40ad00cc4c5b37a33f1b41e630df3e00d52)
h=1
E.set_order(0x3fffffffffffffffffffff015f164053415f93ca0bdd*h)
d=0x1fffffffffffffffffffff80af8b2029a0afc9e505ef
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffefffff155
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
