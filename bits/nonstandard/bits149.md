Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |   1fffff ffffffff ffffffff fffffffe ffffe617 |
| b-value |                                            5 |
| n-value |   1fffff ffffffff fffff622 9e266b6b 5d7e3afb |
+---------+----------------------------------------------+
| x-value |   1c3fff ffffffff ffffffff ffffffff 1dffe920 |
| y-value |   11ba99 651dbe5c cf2960c5 ff73fd92 368f35cc |
+---------+----------------------------------------------+
| (G/2).x |                                            3 |
| (G/2).y |    a40c9 6cfd5e13 561d0652 9b1d3b17 450bfaf0 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffeffffe617
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1c3fffffffffffffffffffffffffff1dffe920,0x11ba99651dbe5ccf2960c5ff73fd92368f35cc)
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
