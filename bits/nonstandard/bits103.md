Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |       7f ffffffff fffffffe ffffabaf |
| b-value |                                   3 |
| n-value |       7f ffffffff fff35978 d1d10771 |
+---------+-------------------------------------+
| x-value |       47 ffffffff ffffffff 6fffd091 |
| y-value |       49 ffffffff ffffffff 6bffcf41 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffeffffabaf
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x47ffffffffffffffff6fffd091,0x49ffffffffffffffff6bffcf41)
h=1
E.set_order(0x7ffffffffffff35978d1d10771*h)
d=0x3ffffffffffff9acbc68e883b9
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffeffffabaf
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
