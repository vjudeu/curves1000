Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  1ffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffea223 |
| b-value |                                                                       7 |
| n-value |  2000000 00000000 00000000 00000000 19a3c9cc 35d1c022 4e1bfa34 4fca9123 |
+---------+-------------------------------------------------------------------------+
| x-value |  14481cd 85689039 b0ad1207 3615a240 e6c2b448 1cd85689 039b0ad0 7e319cd9 |
| y-value |  1de6445 cb0999ec 1000e1e2 07bd5ad1 44394ca0 c279ca00 d2bd2cd8 060f000e |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       4 |
| (G/2).y |   547339 c931558a 2c2b9015 52b41971 29a14846 1e528223 9ca9fc75 9988d585 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffffffffefffea223
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x14481cd85689039b0ad12073615a240e6c2b4481cd85689039b0ad07e319cd9,0x1de6445cb0999ec1000e1e207bd5ad144394ca0c279ca00d2bd2cd8060f000e)
h=1
E.set_order(0x200000000000000000000000000000019a3c9cc35d1c0224e1bfa344fca9123*h)
d=0x10000000000000000000000000000000cd1e4e61ae8e011270dfd1a27e54892
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffffffffefffea223
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
