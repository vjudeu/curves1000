Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |    7ffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff88f |
| b-value |                                                                       3 |
| n-value |    7ffff ffffffff ffffffff ffffffff fa6af71e 0e435c19 dd61c460 ec4a9ee3 |
+---------+-------------------------------------------------------------------------+
| x-value |    47fff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6ffffbcf |
| y-value |     9fff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ebffff6b |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffefffff88f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x47fffffffffffffffffffffffffffffffffffffffffffffffffff6ffffbcf,0x9fffffffffffffffffffffffffffffffffffffffffffffffffffebffff6b)
h=1
E.set_order(0x7fffffffffffffffffffffffffffffa6af71e0e435c19dd61c460ec4a9ee3*h)
d=0x3fffffffffffffffffffffffffffffd357b8f0721ae0ceeb0e23076254f72
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffefffff88f
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
