Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |   1fffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffcfb5 |
| b-value |                                                                       7 |
| n-value |   200000 00000000 00000000 00000000 0b110418 beb66b54 4bb6fe53 3803e1ad |
+---------+-------------------------------------------------------------------------+
| x-value |   11a5a5 a5a5a5a5 a5a5a5a5 a5a5a5a5 a5a5a5a5 a5a5a5a5 a5a5a5a5 18785dd6 |
| y-value |   133d09 4f4f8b3a aa16659a ef4f9a6b eb10cb44 c7b78266 2da7b04f 6179a04e |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       3 |
| (G/2).y |    1a3a8 3847ba36 0d156552 d39bfdea 65173c71 f55a4de4 77382267 60b5ec3b |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffffffeffffcfb5
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x11a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a5a518785dd6,0x133d094f4f8b3aaa16659aef4f9a6beb10cb44c7b782662da7b04f6179a04e)
h=1
E.set_order(0x2000000000000000000000000000000b110418beb66b544bb6fe533803e1ad*h)
d=0x1000000000000000000000000000000588820c5f5b35aa25db7f299c01f0d7
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffffffeffffcfb5
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
