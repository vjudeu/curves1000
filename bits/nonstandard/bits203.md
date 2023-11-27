Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |      7ff ffffffff ffffffff ffffffff ffffffff fffffffe ffff95dd |
| b-value |                                                              7 |
| n-value |      7ff ffffffff ffffffff ffffffb0 01de2cd3 3458c755 66734273 |
+---------+----------------------------------------------------------------+
| x-value |      4cc cccccccc cccccccc cccccccc cccccccc cccccccc 3332f383 |
| y-value |      1cb 928e452d 73e03d76 626a62dc 181ff593 3fafbcfc 4d9e7a9d |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              2 |
| (G/2).y |      2a0 1e8928db aa43746a 257d6c51 4e1a16da 4aec7ef7 c7ae12b1 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffeffff95dd
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x4cccccccccccccccccccccccccccccccccccccccccc3332f383,0x1cb928e452d73e03d76626a62dc181ff5933fafbcfc4d9e7a9d)
h=1
E.set_order(0x7ffffffffffffffffffffffffb001de2cd33458c75566734273*h)
d=0x3ffffffffffffffffffffffffd800ef16699a2c63aab339a13a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffeffff95dd
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
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
