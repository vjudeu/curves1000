Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |    7ffff ffffffff ffffffff fffffffe ffffec0b |
| b-value |                                            5 |
| n-value |    7ffff ffffffff fffffb93 5611bfd2 63824941 |
+---------+----------------------------------------------+
| x-value |    6ffff ffffffff ffffffff ffffffff 1fffee88 |
| y-value |    75988 032a6f5d 2c9b71ff cf0d3e7c e6384994 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |    12a17 3c9bd34e 09deb45d a5aaed1d 798a949f |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffeffffec0b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x6ffffffffffffffffffffffffffff1fffee88,0x75988032a6f5d2c9b71ffcf0d3e7ce6384994)
h=1
E.set_order(0x7fffffffffffffffffb935611bfd263824941*h)
d=0x3fffffffffffffffffdc9ab08dfe931c124a1
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffeffffec0b
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
