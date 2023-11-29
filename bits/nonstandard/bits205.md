Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |     1fff ffffffff ffffffff ffffffff ffffffff fffffffe fffff9af |
| b-value |                                                              3 |
| n-value |     2000 00000000 00000000 00000052 11d69577 520a6a0b 86920027 |
+---------+----------------------------------------------------------------+
| x-value |     11ff ffffffff ffffffff ffffffff ffffffff ffffffff 6ffffc71 |
| y-value |     127f ffffffff ffffffff ffffffff ffffffff ffffffff 6bfffc59 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffefffff9af
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x11ffffffffffffffffffffffffffffffffffffffffff6ffffc71,0x127fffffffffffffffffffffffffffffffffffffffff6bfffc59)
h=1
E.set_order(0x200000000000000000000000005211d69577520a6a0b86920027*h)
d=0x100000000000000000000000002908eb4abba9053505c3490014
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffefffff9af
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
