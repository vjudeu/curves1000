Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |    3ffff ffffffff ffffffff ffffffff fffffffe ffff6317 |
| b-value |                                                     3 |
| n-value |    40000 00000000 00000000 034f1e41 b5bb6f08 38743cdf |
+---------+-------------------------------------------------------+
| x-value |     3fff ffffffff ffffffff ffffffff ffffffff effff630 |
| y-value |    2cfff ffffffff ffffffff ffffffff ffffffff 4bff91ac |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffeffff6317
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x3fffffffffffffffffffffffffffffffffffeffff630,0x2cfffffffffffffffffffffffffffffffffff4bff91ac)
h=1
E.set_order(0x400000000000000000000034f1e41b5bb6f0838743cdf*h)
d=0x20000000000000000000001a78f20daddb7841c3a1e70
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffeffff6317
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
