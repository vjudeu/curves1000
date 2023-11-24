Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |     ffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff2bc7 |
| b-value |                                                                       3 |
| n-value |    10000 00000000 00000000 00000000 01ff982d aff01063 0dd9831d 717c6cd9 |
+---------+-------------------------------------------------------------------------+
| x-value |      fff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff effff2bb |
| y-value |     73ff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 8bff9fd6 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffeffff2bc7
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xfffffffffffffffffffffffffffffffffffffffffffffffffffeffff2bb,0x73ffffffffffffffffffffffffffffffffffffffffffffffffff8bff9fd6)
h=1
E.set_order(0x1000000000000000000000000000001ff982daff010630dd9831d717c6cd9*h)
d=0x800000000000000000000000000000ffcc16d7f8083186ecc18eb8be366d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffeffff2bc7
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
