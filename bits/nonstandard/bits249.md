Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  1ffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffea223 |
| b-value |                                                                       7 |
| n-value |  2000000 00000000 00000000 00000000 19a3c9cc 35d1c022 4e1bfa34 4fca9123 |
+---------+-------------------------------------------------------------------------+
| x-value |  14481cd 85689039 b0ad1207 3615a240 e6c2b448 1cd85689 039b0ad0 7e319cd9 |
| y-value |   219bba 34f66613 efff1e1d f842a52e bbc6b35f 3d8635ff 2d42d326 f9efa215 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       4 |
| (G/2).y |  1ab8cc6 36ceaa75 d3d46fea ad4be68e d65eb7b9 e1ad7ddc 63560389 6675cc9e |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffffffffefffea223
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x14481cd85689039b0ad12073615a240e6c2b4481cd85689039b0ad07e319cd9,0x219bba34f66613efff1e1df842a52ebbc6b35f3d8635ff2d42d326f9efa215)
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
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
