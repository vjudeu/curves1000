Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  3ffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffde23 |
| b-value |                                                                       7 |
| n-value |  3ffffff ffffffff ffffffff ffffffff c0056e9f b4e0adc3 ce8b7d7a 8f7541a5 |
+---------+-------------------------------------------------------------------------+
| x-value |  223d70a 3d70a3d7 0a3d70a3 d70a3d70 a3d70a3d 70a3d70a 3d70a3d6 81479bf8 |
| y-value |  3bd3f4c 633f1fdf e0e1305b d0e1b3ab 7509fd5c 5b5fb4dd 1264fcf8 6fecf4db |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       7 |
| (G/2).y |  36014bb e788a8f6 187e5bfd 0c96f497 b98b0036 995b14de 78c1d15d a97e8d2a |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffffeffffde23
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x223d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d681479bf8,0x3bd3f4c633f1fdfe0e1305bd0e1b3ab7509fd5c5b5fb4dd1264fcf86fecf4db)
h=1
E.set_order(0x3ffffffffffffffffffffffffffffffc0056e9fb4e0adc3ce8b7d7a8f7541a5*h)
d=0x1ffffffffffffffffffffffffffffffe002b74fda7056e1e745bebd47baa0d3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffffeffffde23
modulo_root=(p+1)/4
x=0
b_value=7
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
