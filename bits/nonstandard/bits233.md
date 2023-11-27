Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |      1ff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff104b |
| b-value |                                                                       3 |
| n-value |      1ff ffffffff ffffffff ffffffff ffd5dab0 46491eaf dc62859e 653654b1 |
+---------+-------------------------------------------------------------------------+
| x-value |       9f ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff afffb516 |
| y-value |        7 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff fbfffc41 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffffeffff104b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x9fffffffffffffffffffffffffffffffffffffffffffffffffafffb516,0x7fffffffffffffffffffffffffffffffffffffffffffffffffbfffc41)
h=1
E.set_order(0x1ffffffffffffffffffffffffffffd5dab046491eafdc62859e653654b1*h)
d=0xffffffffffffffffffffffffffffeaed5823248f57ee3142cf329b2a59
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffffeffff104b
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
