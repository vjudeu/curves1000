Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |     1fff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffb83 |
| b-value |                                                                       3 |
| n-value |     2000 00000000 00000000 00000000 0017a85c 4a6664fe 0dbcfe27 fc89a0db |
+---------+-------------------------------------------------------------------------+
| x-value |     19ff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2ffffc59 |
| y-value |     137f ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 63fffd44 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |     1fff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffb81 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffffefffffb83
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x19ffffffffffffffffffffffffffffffffffffffffffffffffff2ffffc59,0x137fffffffffffffffffffffffffffffffffffffffffffffffff63fffd44)
h=1
E.set_order(0x20000000000000000000000000000017a85c4a6664fe0dbcfe27fc89a0db*h)
d=0x1000000000000000000000000000000bd42e2533327f06de7f13fe44d06e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffffefffffb83
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
