Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |      fff ffffffff ffffffff ffffffff fffffffe ffffebe5 |
| b-value |                                                     7 |
| n-value |     1000 00000000 00000000 005cf751 85ac2485 cdf59a71 |
+---------+-------------------------------------------------------+
| x-value |      c64 f52edf8c 9ea5dbf1 93d4bb7e 327a976e fffff06d |
| y-value |      1e1 d710061f 024dae71 f85c9030 bfc17f41 5d24b09c |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     4 |
| (G/2).y |      622 a0151c10 4545f7ec f4739482 02e6cf22 744b993e |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffeffffebe5
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xc64f52edf8c9ea5dbf193d4bb7e327a976efffff06d,0x1e1d710061f024dae71f85c9030bfc17f415d24b09c)
h=1
E.set_order(0x10000000000000000000005cf75185ac2485cdf59a71*h)
d=0x8000000000000000000002e7ba8c2d61242e6facd39
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffeffffebe5
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=0
b_value=7
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    first_y=y_square.powermod(first_modulo_root,p)
    is_on_curve=(first_y.powermod(2,p)==y_square)
    y=first_y
    if not is_on_curve:
        second_y=(first_y*second_multiplier)%p
        is_on_curve=(second_y.powermod(2,p)==y_square)
        y=second_y
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
