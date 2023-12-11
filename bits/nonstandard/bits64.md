Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+---------------------+
| p-value |   fffffffe fffff833 |
| b-value |                   5 |
| n-value | 1 00000000 c4c5d9a1 |
+---------+---------------------+
| x-value |   dfffffff 1ffff92b |
| y-value |   3b1c4e4b 01bb2fe2 |
+---------+---------------------+
| (G/2).x |                   1 |
| (G/2).y |   257f1c3c 40c82881 |
+---------+---------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffefffff833
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xdfffffff1ffff92b,0x3b1c4e4b01bb2fe2)
h=1
E.set_order(0x100000000c4c5d9a1*h)
d=0x800000006262ecd1
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffefffff833
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
