Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |    1ffff ffffffff ffffffff ffffffff fffffffe ffffc523 |
| b-value |                                                     3 |
| n-value |    20000 00000000 00000000 027b808d daebe3a6 c419b513 |
+---------+-------------------------------------------------------+
| x-value |    19fff ffffffff ffffffff ffffffff ffffffff 2fffd02b |
| y-value |    1c7ff ffffffff ffffffff ffffffff ffffffff 1bffcb93 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffeffffc523
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x19fffffffffffffffffffffffffffffffffff2fffd02b,0x1c7ffffffffffffffffffffffffffffffffff1bffcb93)
h=1
E.set_order(0x200000000000000000000027b808ddaebe3a6c419b513*h)
d=0x100000000000000000000013dc046ed75f1d3620cda8a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffeffffc523
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
