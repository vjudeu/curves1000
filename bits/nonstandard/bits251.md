Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  7ffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffe0053 |
| b-value |                                                                       7 |
| n-value |  8000000 00000000 00000000 00000000 3a5b997a 0e4a0f72 f1b795e3 e4471145 |
+---------+-------------------------------------------------------------------------+
| x-value |  1999999 99999999 99999999 99999999 99999999 99999999 99999999 6666000f |
| y-value |  204fe54 25b3f393 161ce7ea 21221a3c 5703962c a5286588 128b5f68 c7217ca8 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |  77fb662 27249df1 3241b1fc d6f06a2c 0b93c9ab 48d02c92 58b11ea7 f31cd59c |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffefffe0053
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x19999999999999999999999999999999999999999999999999999996666000f,0x204fe5425b3f393161ce7ea21221a3c5703962ca5286588128b5f68c7217ca8)
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
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
