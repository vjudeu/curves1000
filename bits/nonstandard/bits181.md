Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |   1fffff ffffffff ffffffff ffffffff fffffffe ffff758b |
| b-value |                                                     7 |
| n-value |   1fffff ffffffff ffffffff ffc6c527 a2df9984 62ff3511 |
+---------+-------------------------------------------------------+
| x-value |    12d2d 2d2d2d2d 2d2d2d2d 2d2d2d2d 2d2d2d2d 23c3beac |
| y-value |   182c6d 19024e6e a431d84a 4e66957f 163d8bd3 d9fd404c |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     3 |
| (G/2).y |    d66b7 819d16e7 be8e6f65 8fb6fd5c 66efdae6 4cc9b44e |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffeffff758b
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x12d2d2d2d2d2d2d2d2d2d2d2d2d2d2d2d2d2d23c3beac,0x182c6d19024e6ea431d84a4e66957f163d8bd3d9fd404c)
h=1
E.set_order(0x1fffffffffffffffffffffffc6c527a2df998462ff3511*h)
d=0xfffffffffffffffffffffffe36293d16fccc2317f9a89
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffeffff758b
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
