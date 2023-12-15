Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value | 1fffffff fffffffe ffffe71f |
| b-value |                          5 |
| n-value | 20000000 00002795 7b951207 |
+---------+----------------------------+
| x-value | 11ffffff ffffffff 6ffff204 |
| y-value | 1a7fffff ffffffff 2bffeb61 |
+---------+----------------------------+
| (G/2).x | 1fffffff fffffffe ffffe71e |
| (G/2).y |                          2 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffeffffe71f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x11ffffffffffffff6ffff204,0x1a7fffffffffffff2bffeb61)
h=1
E.set_order(0x20000000000027957b951207*h)
d=0x10000000000013cabdca8904
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffeffffe71f
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
