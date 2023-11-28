Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff76d |
| b-value |                                                                       5 |
| n-value | 40000000 00000000 00000000 00000000 f5dfe982 e92ec378 aab02ef2 e75f6507 |
+---------+-------------------------------------------------------------------------+
| x-value | 3b13b13b 13b13b13 b13b13b1 3b13b13b 13b13b13 b13b13b1 3b13b13a 27626e3c |
| y-value | 32f3f7a7 5d729cad a0ff7fc3 bbaf6756 17fcbfbc 74003f5e d524e6b3 0d631e6d |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |  b37b151 b1ef890b 2c0e66d8 90a900ac 3702a229 5b3b1b3f 8cfaf180 8735d20f |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffffffefffff76d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x3b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13a27626e3c,0x32f3f7a75d729cada0ff7fc3bbaf675617fcbfbc74003f5ed524e6b30d631e6d)
h=1
E.set_order(0x40000000000000000000000000000000f5dfe982e92ec378aab02ef2e75f6507*h)
d=0x200000000000000000000000000000007aeff4c1749761bc5558177973afb284
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffffffefffff76d
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
