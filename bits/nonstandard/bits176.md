Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |     ffff ffffffff ffffffff ffffffff fffffffe ffff8d3b |
| b-value |                                                     3 |
| n-value |    10000 00000000 00000000 0165d757 b2ce6bc7 c9a94fcb |
+---------+-------------------------------------------------------+
| x-value |     4fff ffffffff ffffffff ffffffff ffffffff afffdc21 |
| y-value |     c3ff ffffffff ffffffff ffffffff ffffffff 3bffa821 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffeffff8d3b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x4fffffffffffffffffffffffffffffffffffafffdc21,0xc3ffffffffffffffffffffffffffffffffff3bffa821)
h=1
E.set_order(0x1000000000000000000000165d757b2ce6bc7c9a94fcb*h)
d=0x8000000000000000000000b2ebabd96735e3e4d4a7e6
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffeffff8d3b
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
