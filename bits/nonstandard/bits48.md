Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |     fffe fffffcfb |
| b-value |                 7 |
| n-value |     fffe fe28e6bd |
+---------+-------------------+
| x-value |     cccb fffffd94 |
| y-value |      c69 24072ed2 |
+---------+-------------------+
| (G/2).x |                 2 |
| (G/2).y |     1c34 c63edec6 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0xfffefffffcfb
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xcccbfffffd94,0xc6924072ed2)
h=1
E.set_order(0xfffefe28e6bd*h)
d=0x7fff7f14735f
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffefffffcfb
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
