Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |   3fffff ffffffff ffffffff fffffffe ffffff6b |
| b-value |                                            3 |
| n-value |   3fffff ffffffff fffff1bd 73579b0d af35ccad |
+---------+----------------------------------------------+
| x-value |   13ffff ffffffff ffffffff ffffffff afffffd0 |
| y-value |   20ffff ffffffff ffffffff ffffffff 7bffffb3 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffeffffff6b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x13ffffffffffffffffffffffffffffafffffd0,0x20ffffffffffffffffffffffffffff7bffffb3)
h=1
E.set_order(0x3ffffffffffffffffff1bd73579b0daf35ccad*h)
d=0x1ffffffffffffffffff8deb9abcd86d79ae657
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffeffffff6b
modulo_root=(p+1)/4
x=0
b_value=3
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
