Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |       ff ffffffff ffffffff ffffffff fffffffe ffffe267 |
| b-value |                                                     5 |
| n-value |       ff ffffffff ffffffff fff0609c ded7c950 9486e763 |
+---------+-------------------------------------------------------+
| x-value |       81 ffffffff ffffffff ffffffff ffffffff 7dfff0f8 |
| y-value |       2f 63a018e5 79868e12 3d431f0d 36b0d088 493ac098 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     3 |
| (G/2).y |       19 b91f901b d7ca3a5f 961c5f95 fcd63953 26b78188 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffeffffe267
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x81ffffffffffffffffffffffffffffffff7dfff0f8,0x2f63a018e579868e123d431f0d36b0d088493ac098)
h=1
E.set_order(0xfffffffffffffffffffff0609cded7c9509486e763*h)
d=0x7ffffffffffffffffffff8304e6f6be4a84a4373b2
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffeffffe267
modulo_root=(p+1)/4
x=0
b_value=5
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
