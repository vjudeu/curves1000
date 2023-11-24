Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |   3fffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffef1e3 |
| b-value |                                                                       3 |
| n-value |   3fffff ffffffff ffffffff ffffffff f0000a73 1b8a0fe8 f44fe724 ec51a9a7 |
+---------+-------------------------------------------------------------------------+
| x-value |   33ffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2fff2487 |
| y-value |    6ffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff e3ffe275 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |   3fffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffef1e1 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffffefffef1e3
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x33ffffffffffffffffffffffffffffffffffffffffffffffffffff2fff2487,0x6ffffffffffffffffffffffffffffffffffffffffffffffffffffe3ffe275)
h=1
E.set_order(0x3ffffffffffffffffffffffffffffff0000a731b8a0fe8f44fe724ec51a9a7*h)
d=0x1ffffffffffffffffffffffffffffff80005398dc507f47a27f3927628d4d4
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffffefffef1e3
modulo_root=(p+1)/4
x=0
b_value=3
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
