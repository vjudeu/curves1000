Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |       ff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff8c3f |
| b-value |                                                                       3 |
| n-value |       ff ffffffff ffffffff ffffffff ffe20800 5d2950ad 978ff600 e7e10aab |
+---------+-------------------------------------------------------------------------+
| x-value |       8f ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6fffbee2 |
| y-value |       d3 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2bffa024 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffffeffff8c3f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x8fffffffffffffffffffffffffffffffffffffffffffffffff6fffbee2,0xd3ffffffffffffffffffffffffffffffffffffffffffffffff2bffa024)
h=1
E.set_order(0xffffffffffffffffffffffffffffe208005d2950ad978ff600e7e10aab*h)
d=0x7ffffffffffffffffffffffffffff104002e94a856cbc7fb0073f08556
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffffeffff8c3f
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
