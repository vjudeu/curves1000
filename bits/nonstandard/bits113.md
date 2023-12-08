Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |    1ffff ffffffff fffffffe fffffcbb |
| b-value |                                   3 |
| n-value |    1ffff ffffffff fe7b89ac c5c1749b |
+---------+-------------------------------------+
| x-value |     9fff ffffffff ffffffff affffef9 |
| y-value |    187ff ffffffff ffffffff 3bfffd7f |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffefffffcbb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x9fffffffffffffffffffaffffef9,0x187ffffffffffffffffff3bfffd7f)
h=1
E.set_order(0x1fffffffffffffe7b89acc5c1749b*h)
d=0xffffffffffffff3dc4d662e0ba4e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffefffffcbb
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
