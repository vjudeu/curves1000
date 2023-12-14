Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |      1ff ffffffff ffffffff ffffffff fffffffe ffffe923 |
| b-value |                                                     5 |
| n-value |      1ff ffffffff ffffffff ffd5f032 20b2b891 54839423 |
+---------+-------------------------------------------------------+
| x-value |      19f ffffffff ffffffff ffffffff ffffffff 2fffed6f |
| y-value |       c7 ffffffff ffffffff ffffffff ffffffff 9bfff70d |
+---------+-------------------------------------------------------+
| (G/2).x |      1ff ffffffff ffffffff ffffffff fffffffe ffffe922 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffeffffe923
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x19fffffffffffffffffffffffffffffffff2fffed6f,0xc7ffffffffffffffffffffffffffffffff9bfff70d)
h=1
E.set_order(0x1ffffffffffffffffffffd5f03220b2b89154839423*h)
d=0xffffffffffffffffffffeaf81910595c48aa41ca12
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffeffffe923
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
