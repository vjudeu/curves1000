Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |      1ff ffffffff ffffffff fffffffe fffff0df |
| b-value |                                            5 |
| n-value |      200 00000000 00000021 ac389041 dc338a65 |
+---------+----------------------------------------------+
| x-value |      11f ffffffff ffffffff ffffffff 6ffff780 |
| y-value |      1a7 ffffffff ffffffff ffffffff 2bfff374 |
+---------+----------------------------------------------+
| (G/2).x |      1ff ffffffff ffffffff fffffffe fffff0de |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffefffff0df
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x11fffffffffffffffffffffffff6ffff780,0x1a7ffffffffffffffffffffffff2bfff374)
h=1
E.set_order(0x2000000000000000021ac389041dc338a65*h)
d=0x1000000000000000010d61c4820ee19c533
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffefffff0df
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
