Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |      1ff ffffffff ffffffff ffffffff ffffffff fffffffe fffff133 |
| b-value |                                                              3 |
| n-value |      1ff ffffffff ffffffff ffffffdc e7219098 526b84be bdcf14c9 |
+---------+----------------------------------------------------------------+
| x-value |      19f ffffffff ffffffff ffffffff ffffffff ffffffff 2ffff3f8 |
| y-value |       47 ffffffff ffffffff ffffffff ffffffff ffffffff dbfffdeb |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffefffff133
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x19fffffffffffffffffffffffffffffffffffffffff2ffff3f8,0x47ffffffffffffffffffffffffffffffffffffffffdbfffdeb)
h=1
E.set_order(0x1ffffffffffffffffffffffffdce7219098526b84bebdcf14c9*h)
d=0xffffffffffffffffffffffffee7390c84c2935c25f5ee78a65
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffefffff133
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
