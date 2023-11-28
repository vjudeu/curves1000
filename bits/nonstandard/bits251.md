Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  7ffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffe0053 |
| b-value |                                                                       7 |
| n-value |  8000000 00000000 00000000 00000000 3a5b997a 0e4a0f72 f1b795e3 e4471145 |
+---------+-------------------------------------------------------------------------+
| x-value |  1999999 99999999 99999999 99999999 99999999 99999999 99999999 6666000f |
| y-value |  5fb01ab da4c0c6c e9e31815 dedde5c3 a8fc69d3 5ad79a77 ed74a096 38dc83ab |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |   80499d d8db620e cdbe4e03 290f95d3 f46c3654 b72fd36d a74ee157 0ce12ab7 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffefffe0053
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x19999999999999999999999999999999999999999999999999999996666000f,0x5fb01abda4c0c6ce9e31815dedde5c3a8fc69d35ad79a77ed74a09638dc83ab)
h=1
E.set_order(0x80000000000000000000000000000003a5b997a0e4a0f72f1b795e3e4471145*h)
d=0x40000000000000000000000000000001d2dccbd072507b978dbcaf1f22388a3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffefffe0053
modulo_root=(p+1)/4
x=0
b_value=7
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
