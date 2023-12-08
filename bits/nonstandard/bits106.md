Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |      3ff ffffffff fffffffe fffffe3f |
| b-value |                                   7 |
| n-value |      400 00000000 002ab79f 08bdf16f |
+---------+-------------------------------------+
| x-value |      11f ffffffff ffffffff b7ffff80 |
| y-value |      13e 7b71976d 532deaeb 91087905 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |       69 168d950e a2d24583 b784a3e5 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffefffffe3f
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x11fffffffffffffffffb7ffff80,0x13e7b71976d532deaeb91087905)
h=1
E.set_order(0x40000000000002ab79f08bdf16f*h)
d=0x2000000000000155bcf845ef8b8
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffefffffe3f
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
