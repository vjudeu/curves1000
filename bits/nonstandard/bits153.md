Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |  1ffffff ffffffff ffffffff fffffffe ffffc23b |
| b-value |                                            5 |
| n-value |  2000000 00000000 00001d86 1a848b75 9f05fb3f |
+---------+----------------------------------------------+
| x-value |   9fffff ffffffff ffffffff ffffffff afffecb5 |
| y-value |   87ffff ffffffff ffffffff ffffffff bbffef93 |
+---------+----------------------------------------------+
| (G/2).x |  1ffffff ffffffff ffffffff fffffffe ffffc23a |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffeffffc23b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x9fffffffffffffffffffffffffffffafffecb5,0x87ffffffffffffffffffffffffffffbbffef93)
h=1
E.set_order(0x20000000000000000001d861a848b759f05fb3f*h)
d=0x10000000000000000000ec30d4245bacf82fda0
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffeffffc23b
modulo_root=(p+1)/4
x=p
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
        x-=1
```
