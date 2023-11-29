Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |    fffff ffffffff ffffffff ffffffff fffffffe ffffcf23 |
| b-value |                                                     3 |
| n-value |   100000 00000000 00000000 06f484f2 c8c89913 fedd8475 |
+---------+-------------------------------------------------------+
| x-value |    cffff ffffffff ffffffff ffffffff ffffffff 2fffd84b |
| y-value |    e3fff ffffffff ffffffff ffffffff ffffffff 1bffd47b |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffeffffcf23
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xcffffffffffffffffffffffffffffffffffff2fffd84b,0xe3fffffffffffffffffffffffffffffffffff1bffd47b)
h=1
E.set_order(0x100000000000000000000006f484f2c8c89913fedd8475*h)
d=0x800000000000000000000037a427964644c89ff6ec23b
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffeffffcf23
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
