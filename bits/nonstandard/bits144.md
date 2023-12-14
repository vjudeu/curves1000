Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |     ffff ffffffff ffffffff fffffffe ffffafdf |
| b-value |                                            5 |
| n-value |    10000 00000000 000001fb 925b1ea5 81d85f49 |
+---------+----------------------------------------------+
| x-value |     8fff ffffffff ffffffff ffffffff 6fffd2f0 |
| y-value |     d3ff ffffffff ffffffff ffffffff 2bffbda0 |
+---------+----------------------------------------------+
| (G/2).x |     ffff ffffffff ffffffff fffffffe ffffafde |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffeffffafdf
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x8fffffffffffffffffffffffffff6fffd2f0,0xd3ffffffffffffffffffffffffff2bffbda0)
h=1
E.set_order(0x1000000000000000001fb925b1ea581d85f49*h)
d=0x800000000000000000fdc92d8f52c0ec2fa5
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffeffffafdf
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
