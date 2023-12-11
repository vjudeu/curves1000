Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |   3fffff fffffffe ffffee97 |
| b-value |                          7 |
| n-value |   400000 00000ffe 6da917c3 |
+---------+----------------------------+
| x-value |    1ffff ffffffff f7ffff73 |
| y-value |   1cf3b3 163f29f9 28c37a30 |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |    a8d5c a2af1782 28424465 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffeffffee97
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x1fffffffffffff7ffff73,0x1cf3b3163f29f928c37a30)
h=1
E.set_order(0x40000000000ffe6da917c3*h)
d=0x200000000007ff36d48be2
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffeffffee97
modulo_root=(p+1)/4
x=0
b_value=7
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
