Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |        1 fffffcdb |
| b-value |                 5 |
| n-value |        2 00024263 |
+---------+-------------------+
| x-value |        1 bffffd3e |
| y-value |           39bde17 |
+---------+-------------------+
| (G/2).x |                 1 |
| (G/2).y |          afaeed24 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffcdb
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1bffffd3e,0x39bde17)
h=1
E.set_order(0x200024263*h)
d=0x100012132
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffcdb
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
