Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |       1f ffffffff ffffffff ffffffff ffffffff fffffffe ffff575b |
| b-value |                                                              5 |
| n-value |       20 00000000 00000000 00000005 01a23bf2 1319ab20 c1eaa8fb |
+---------+----------------------------------------------------------------+
| x-value |        9 ffffffff ffffffff ffffffff ffffffff ffffffff afffcb4f |
| y-value |       18 7fffffff ffffffff ffffffff ffffffff ffffffff 3bff7edd |
+---------+----------------------------------------------------------------+
| (G/2).x |       1f ffffffff ffffffff ffffffff ffffffff fffffffe ffff575a |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffeffff575b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x9ffffffffffffffffffffffffffffffffffffffffafffcb4f,0x187fffffffffffffffffffffffffffffffffffffff3bff7edd)
h=1
E.set_order(0x2000000000000000000000000501a23bf21319ab20c1eaa8fb*h)
d=0x1000000000000000000000000280d11df9098cd59060f5547e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffeffff575b
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
