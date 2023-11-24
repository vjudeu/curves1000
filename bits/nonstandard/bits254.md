Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff76d |
| b-value |                                                                       5 |
| n-value | 40000000 00000000 00000000 00000000 f5dfe982 e92ec378 aab02ef2 e75f6507 |
+---------+-------------------------------------------------------------------------+
| x-value | 3b13b13b 13b13b13 b13b13b1 3b13b13b 13b13b13 b13b13b1 3b13b13a 27626e3c |
| y-value |  d0c0858 a28d6352 5f00803c 445098a9 e8034043 8bffc0a1 2adb194b f29cd900 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y | 34c84eae 4e1076f4 d3f19927 6f56ff53 c8fd5dd6 a4c4e4c0 73050e7e 78ca255e |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffffffefffff76d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x3b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13b13a27626e3c,0xd0c0858a28d63525f00803c445098a9e80340438bffc0a12adb194bf29cd900)
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
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
