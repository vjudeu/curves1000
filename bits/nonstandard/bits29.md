Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------+
| p-value | 1fffff87 |
| b-value |        7 |
| n-value | 20009e03 |
+---------+----------+
| x-value | 10ffffbe |
| y-value |  e8b9749 |
+---------+----------+
| (G/2).x |        1 |
| (G/2).y |  ba2ffd4 |
+---------+----------+
```
Sage code for testing half of the generator:
```
p=0x1fffff87
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x10ffffbe,0xe8b9749)
h=1
E.set_order(0x20009e03*h)
d=0x10004f02
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffff87
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
