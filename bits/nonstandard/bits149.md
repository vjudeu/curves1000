Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |   1fffff ffffffff ffffffff fffffffe ffffe617 |
| b-value |                                            5 |
| n-value |   1fffff ffffffff fffff622 9e266b6b 5d7e3afb |
+---------+----------------------------------------------+
| x-value |    1ffff ffffffff ffffffff ffffffff effffe64 |
| y-value |    67fff ffffffff ffffffff ffffffff cbfffab8 |
+---------+----------------------------------------------+
| (G/2).x |   1fffff ffffffff ffffffff fffffffe ffffe616 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffeffffe617
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1ffffffffffffffffffffffffffffeffffe64,0x67fffffffffffffffffffffffffffcbfffab8)
h=1
E.set_order(0x1ffffffffffffffffff6229e266b6b5d7e3afb*h)
d=0xffffffffffffffffffb114f1335b5aebf1d7e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffeffffe617
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
