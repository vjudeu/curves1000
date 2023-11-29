Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |     ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff7f37 |
| b-value |                                                              3 |
| n-value |     ffff ffffffff ffffffff ffffff28 e982cafa f513cfa3 b950748d |
+---------+----------------------------------------------------------------+
| x-value |      fff ffffffff ffffffff ffffffff ffffffff ffffffff effff7f2 |
| y-value |     33ff ffffffff ffffffff ffffffff ffffffff ffffffff cbffe5d7 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffeffff7f37
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xfffffffffffffffffffffffffffffffffffffffffffeffff7f2,0x33ffffffffffffffffffffffffffffffffffffffffffcbffe5d7)
h=1
E.set_order(0xffffffffffffffffffffffffff28e982cafaf513cfa3b950748d*h)
d=0x7fffffffffffffffffffffffff9474c1657d7a89e7d1dca83a47
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffeffff7f37
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
