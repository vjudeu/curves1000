Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  3ffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffde23 |
| b-value |                                                                       7 |
| n-value |  3ffffff ffffffff ffffffff ffffffff c0056e9f b4e0adc3 ce8b7d7a 8f7541a5 |
+---------+-------------------------------------------------------------------------+
| x-value |  223d70a 3d70a3d7 0a3d70a3 d70a3d70 a3d70a3d 70a3d70a 3d70a3d6 81479bf8 |
| y-value |   42c0b3 9cc0e020 1f1ecfa4 2f1e4c54 8af602a3 a4a04b22 ed9b0306 9012e948 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       7 |
| (G/2).y |   9feb44 18775709 e781a402 f3690b68 4674ffc9 66a4eb21 873e2ea1 568150f9 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffffeffffde23
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x223d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d681479bf8,0x42c0b39cc0e0201f1ecfa42f1e4c548af602a3a4a04b22ed9b03069012e948)
h=1
E.set_order(0x3ffffffffffffffffffffffffffffffc0056e9fb4e0adc3ce8b7d7a8f7541a5*h)
d=0x1ffffffffffffffffffffffffffffffe002b74fda7056e1e745bebd47baa0d3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffffeffffde23
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
