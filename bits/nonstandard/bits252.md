Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff79f7 |
| b-value |                                                                       5 |
| n-value |  fffffff ffffffff ffffffff ffffffff 80d4ce69 52c22581 bf54a584 dee77b43 |
+---------+-------------------------------------------------------------------------+
| x-value |  c4ec4ec 4ec4ec4e c4ec4ec4 ec4ec4ec 4ec4ec4e c4ec4ec4 ec4ec4eb 89d8366e |
| y-value |  9d595f3 66abfcb0 d1059640 fa4a9dcc 11ca6848 fc0fbb13 2fd1f129 e5b11c8a |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |  4378499 3d01a911 a999415c 3525cf2d 64fed456 eba4a817 a5395a15 86945294 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffffffeffff79f7
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xc4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4eb89d8366e,0x9d595f366abfcb0d1059640fa4a9dcc11ca6848fc0fbb132fd1f129e5b11c8a)
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
x=0
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
        x+=1
```
