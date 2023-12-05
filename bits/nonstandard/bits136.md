Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |       ff ffffffff ffffffff fffffffe ffff98c3 |
| b-value |                                            7 |
| n-value |       ff ffffffff ffffffe2 84dedfe6 ee4bb1cb |
+---------+----------------------------------------------+
| x-value |       eb 4b4b4b4b 4b4b4b4b 4b4b4b4a 5fffa11c |
| y-value |       c7 b9838a6c 7de5b9f7 d0ef005f b3999420 |
+---------+----------------------------------------------+
| (G/2).x |                                            3 |
| (G/2).y |       11 0a7d3a2c 99b6edc9 6d5c15fc e95f2285 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffeffff98c3
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xeb4b4b4b4b4b4b4b4b4b4b4b4a5fffa11c,0xc7b9838a6c7de5b9f7d0ef005fb3999420)
h=1
E.set_order(0xffffffffffffffffe284dedfe6ee4bb1cb*h)
d=0x7ffffffffffffffff1426f6ff37725d8e6
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffeffff98c3
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
