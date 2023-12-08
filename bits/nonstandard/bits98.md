Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |        3 ffffffff fffffffe fffff9a7 |
| b-value |                                   3 |
| n-value |        4 00000000 000014f4 c934e1fb |
+---------+-------------------------------------+
| x-value |          3fffffff ffffffff efffff99 |
| y-value |        3 cfffffff ffffffff 0bfff9f3 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffefffff9a7
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x3fffffffffffffffefffff99,0x3cfffffffffffffff0bfff9f3)
h=1
E.set_order(0x400000000000014f4c934e1fb*h)
d=0x20000000000000a7a649a70fe
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffefffff9a7
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
