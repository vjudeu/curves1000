Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |    fffff ffffffff fffffffe fffff167 |
| b-value |                                   5 |
| n-value |   100000 00000000 066b0c79 9cf0477f |
+---------+-------------------------------------+
| x-value |     ffff ffffffff ffffffff efffff19 |
| y-value |    73fff ffffffff ffffffff 8bfff95e |
+---------+-------------------------------------+
| (G/2).x |    fffff ffffffff fffffffe fffff166 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffefffff167
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xffffffffffffffffffffefffff19,0x73fffffffffffffffffff8bfff95e)
h=1
E.set_order(0x10000000000000066b0c799cf0477f*h)
d=0x80000000000000335863cce7823c0
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffefffff167
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
