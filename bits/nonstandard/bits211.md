Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    7ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffbf05 |
| b-value |                                                              5 |
| n-value |    80000 00000000 00000000 00000591 a0eec5d5 21871f7f 36a8d2cb |
+---------+----------------------------------------------------------------+
| x-value |    5e9bd 37a6f4de 9bd37a6f 4de9bd37 a6f4de9b d37a6f4d 2c8560ab |
| y-value |    6e710 95c748ae d7bd55bd c207fc97 ec2291f0 4773843f f06006b5 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              4 |
| (G/2).y |    26e0d 047b2b4f 4ca18dbd f8bea9ff e17a83b3 f10c0084 014751cc |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffeffffbf05
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x5e9bd37a6f4de9bd37a6f4de9bd37a6f4de9bd37a6f4d2c8560ab,0x6e71095c748aed7bd55bdc207fc97ec2291f04773843ff06006b5)
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
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
