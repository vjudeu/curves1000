Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |       3f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff7df |
| b-value |                                                                       5 |
| n-value |       40 00000000 00000000 00000000 0002202e 64b91c4e cccb3277 dd18b89f |
+---------+-------------------------------------------------------------------------+
| x-value |       1c 7fffffff ffffffff ffffffff ffffffff ffffffff ffffffff 8dfffc61 |
| y-value |       18 dcf167c7 7a58b3e2 2f6d7b69 155ddcf6 8dab2b2c 284690fc 465b2d2f |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       3 |
| (G/2).y |       30 54d85b68 0a649143 f046fda1 dde3451e 80122fd9 13549a67 84971708 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffefffff7df
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1c7fffffffffffffffffffffffffffffffffffffffffffffff8dfffc61,0x18dcf167c77a58b3e22f6d7b69155ddcf68dab2b2c284690fc465b2d2f)
h=1
E.set_order(0x400000000000000000000000000002202e64b91c4ecccb3277dd18b89f*h)
d=0x2000000000000000000000000000011017325c8e276665993bee8c5c50
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffefffff7df
modulo_root=(p+1)/4
x=0
b_value=5
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
