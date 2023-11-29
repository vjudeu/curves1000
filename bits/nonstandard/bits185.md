Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |  1ffffff ffffffff ffffffff ffffffff fffffffe ffffd21f |
| b-value |                                                     3 |
| n-value |  1ffffff ffffffff ffffffff d2beee92 29e9d6b8 1eb7b345 |
+---------+-------------------------------------------------------+
| x-value |  11fffff ffffffff ffffffff ffffffff ffffffff 6fffe630 |
| y-value |   a7ffff ffffffff ffffffff ffffffff ffffffff abfff0f2 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffeffffd21f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x11fffffffffffffffffffffffffffffffffffff6fffe630,0xa7ffffffffffffffffffffffffffffffffffffabfff0f2)
h=1
E.set_order(0x1ffffffffffffffffffffffd2beee9229e9d6b81eb7b345*h)
d=0xffffffffffffffffffffffe95f774914f4eb5c0f5bd9a3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffeffffd21f
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
