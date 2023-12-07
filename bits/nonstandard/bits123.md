Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |  7ffffff ffffffff fffffffe ffffe74f |
| b-value |                                   7 |
| n-value |  7ffffff ffffffff c988d15f 758dbe7b |
+---------+-------------------------------------+
| x-value |  63fffff ffffffff ffffffff 37ffecb4 |
| y-value |  5c93331 f2b47a3c 0e2b4244 b8d80e04 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |   ea2c7a 7f12678d a7f1316c 1bdb0493 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffeffffe74f
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x63fffffffffffffffffffff37ffecb4,0x5c93331f2b47a3c0e2b4244b8d80e04)
h=1
E.set_order(0x7ffffffffffffffc988d15f758dbe7b*h)
d=0x3ffffffffffffffe4c468afbac6df3e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffeffffe74f
modulo_root=(p+1)/4
x=0
b_value=7
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
