Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    7ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffbf05 |
| b-value |                                                              5 |
| n-value |    80000 00000000 00000000 00000591 a0eec5d5 21871f7f 36a8d2cb |
+---------+----------------------------------------------------------------+
| x-value |    5e9bd 37a6f4de 9bd37a6f 4de9bd37 a6f4de9b d37a6f4d 2c8560ab |
| y-value |    118ef 6a38b751 2842aa42 3df80368 13dd6e0f b88c7bbf 0f9fb850 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              4 |
| (G/2).y |    591f2 fb84d4b0 b35e7242 07415600 1e857c4c 0ef3ff7a feb86d39 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffeffffbf05
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x5e9bd37a6f4de9bd37a6f4de9bd37a6f4de9bd37a6f4d2c8560ab,0x118ef6a38b7512842aa423df8036813dd6e0fb88c7bbf0f9fb850)
h=1
E.set_order(0x80000000000000000000000000591a0eec5d521871f7f36a8d2cb*h)
d=0x400000000000000000000000002c8d07762ea90c38fbf9b546966
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffeffffbf05
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
