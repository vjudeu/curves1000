Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |      7ff ffffffff fffffffe fffff433 |
| b-value |                                   3 |
| n-value |      800 00000000 005a546a 9233ab11 |
+---------+-------------------------------------+
| x-value |      67f ffffffff ffffffff 2ffff668 |
| y-value |      11f ffffffff ffffffff dbfffe57 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffefffff433
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x67fffffffffffffffff2ffff668,0x11fffffffffffffffffdbfffe57)
h=1
E.set_order(0x80000000000005a546a9233ab11*h)
d=0x40000000000002d2a354919d589
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffefffff433
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
