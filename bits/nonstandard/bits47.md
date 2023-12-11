Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |     7ffe fffffefb |
| b-value |                 3 |
| n-value |     7fff 015691c5 |
+---------+-------------------+
| x-value |     27ff afffffad |
| y-value |     61ff 3bffff38 |
+---------+-------------------+
| (G/2).x |                 1 |
| (G/2).y |                 2 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffefffffefb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x27ffafffffad,0x61ff3bffff38)
h=1
E.set_order(0x7fff015691c5*h)
d=0x3fff80ab48e3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffefffffefb
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
