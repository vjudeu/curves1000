Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |     1fff ffffffff ffffffff ffffffff fffffffe fffff2fb |
| b-value |                                                     3 |
| n-value |     1fff ffffffff ffffffff ff4d0ca7 5340f574 4a07f3f7 |
+---------+-------------------------------------------------------+
| x-value |      9ff ffffffff ffffffff ffffffff ffffffff affffbed |
| y-value |     187f ffffffff ffffffff ffffffff ffffffff 3bfff608 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffefffff2fb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x9ffffffffffffffffffffffffffffffffffaffffbed,0x187fffffffffffffffffffffffffffffffff3bfff608)
h=1
E.set_order(0x1fffffffffffffffffffff4d0ca75340f5744a07f3f7*h)
d=0xfffffffffffffffffffffa68653a9a07aba2503f9fc
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffefffff2fb
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
