Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |      3ff ffffffff ffffffff fffffffe fffffaeb |
| b-value |                                            3 |
| n-value |      3ff ffffffff ffffffc1 a2b9c5af 66beefb5 |
+---------+----------------------------------------------+
| x-value |      13f ffffffff ffffffff ffffffff affffe68 |
| y-value |      20f ffffffff ffffffff ffffffff 7bfffd61 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffefffffaeb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x13fffffffffffffffffffffffffaffffe68,0x20fffffffffffffffffffffffff7bfffd61)
h=1
E.set_order(0x3ffffffffffffffffc1a2b9c5af66beefb5*h)
d=0x1ffffffffffffffffe0d15ce2d7b35f77db
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffefffffaeb
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
