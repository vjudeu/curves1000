Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |   7fffff ffffffff ffffffff ffffffff fffffffe ffffc62b |
| b-value |                                                     3 |
| n-value |   800000 00000000 00000000 16509571 f6d67e25 f7986d55 |
+---------+-------------------------------------------------------+
| x-value |   27ffff ffffffff ffffffff ffffffff ffffffff afffedec |
| y-value |   41ffff ffffffff ffffffff ffffffff ffffffff 7bffe22e |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffeffffc62b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x27ffffffffffffffffffffffffffffffffffffafffedec,0x41ffffffffffffffffffffffffffffffffffff7bffe22e)
h=1
E.set_order(0x800000000000000000000016509571f6d67e25f7986d55*h)
d=0x40000000000000000000000b284ab8fb6b3f12fbcc36ab
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffeffffc62b
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
