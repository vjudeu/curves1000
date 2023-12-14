Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff79f7 |
| b-value |                                                                       5 |
| n-value |  fffffff ffffffff ffffffff ffffffff 80d4ce69 52c22581 bf54a584 dee77b43 |
+---------+-------------------------------------------------------------------------+
| x-value |   ffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff effff7a2 |
| y-value |  b3fffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 4bffa1bd |
+---------+-------------------------------------------------------------------------+
| (G/2).x |  fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff79f6 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffffffeffff79f7
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xffffffffffffffffffffffffffffffffffffffffffffffffffffffeffff7a2,0xb3fffffffffffffffffffffffffffffffffffffffffffffffffffff4bffa1bd)
h=1
E.set_order(0xfffffffffffffffffffffffffffffff80d4ce6952c22581bf54a584dee77b43*h)
d=0x7ffffffffffffffffffffffffffffffc06a6734a96112c0dfaa52c26f73bda2
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffffffeffff79f7
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
