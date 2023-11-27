Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |       1f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffe84b |
| b-value |                                                                       3 |
| n-value |       1f ffffffff ffffffff ffffffff fffe9519 84048858 d78abdbc 18c1620f |
+---------+-------------------------------------------------------------------------+
| x-value |        9 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff affff896 |
| y-value |          7fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fbffffa1 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffeffffe84b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x9ffffffffffffffffffffffffffffffffffffffffffffffffaffff896,0x7ffffffffffffffffffffffffffffffffffffffffffffffffbffffa1)
h=1
E.set_order(0x1ffffffffffffffffffffffffffffe951984048858d78abdbc18c1620f*h)
d=0xfffffffffffffffffffffffffffff4a8cc202442c6bc55ede0c60b108
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffeffffe84b
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
