Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |    7fffe fffffd9f |
| b-value |                 5 |
| n-value |    7fffe fab81d7b |
+---------+-------------------+
| x-value |    1d89d 4ec4ebc1 |
| y-value |     1dcb 3691dd6d |
+---------+-------------------+
| (G/2).x |                 2 |
| (G/2).y |    39c46 fcfdd146 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffefffffd9f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1d89d4ec4ebc1,0x1dcb3691dd6d)
h=1
E.set_order(0x7fffefab81d7b*h)
d=0x3ffff7d5c0ebe
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffefffffd9f
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
