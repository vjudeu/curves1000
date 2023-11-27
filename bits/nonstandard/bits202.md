Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |      3ff ffffffff ffffffff ffffffff ffffffff fffffffe ffffcc6b |
| b-value |                                                              5 |
| n-value |      400 00000000 00000000 00000032 7ee367c0 a5dcdd29 77ad66fd |
+---------+----------------------------------------------------------------+
| x-value |      37f ffffffff ffffffff ffffffff ffffffff ffffffff 1fffd2dc |
| y-value |      3bd 4b19020a f77672f1 f696f762 76d36daf 80070dcd 2206af42 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              2 |
| (G/2).y |      293 829ffa0e a478e028 d58ea4b3 036a63d2 e8a5a94a cb8ec905 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffeffffcc6b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x37fffffffffffffffffffffffffffffffffffffffff1fffd2dc,0x3bd4b19020af77672f1f696f76276d36daf80070dcd2206af42)
h=1
E.set_order(0x4000000000000000000000000327ee367c0a5dcdd2977ad66fd*h)
d=0x2000000000000000000000000193f71b3e052ee6e94bbd6b37f
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffeffffcc6b
modulo_root=(p+1)/4
x=0
b_value=5
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
