Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |   ffffff ffffffff ffffffff fffffffe ffffec93 |
| b-value |                                            3 |
| n-value |  1000000 00000000 000014cf e4f6780c 653a6889 |
+---------+----------------------------------------------+
| x-value |   cfffff ffffffff ffffffff ffffffff 2ffff036 |
| y-value |   a3ffff ffffffff ffffffff ffffffff 5bfff38e |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffeffffec93
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xcfffffffffffffffffffffffffffff2ffff036,0xa3ffffffffffffffffffffffffffff5bfff38e)
h=1
E.set_order(0x100000000000000000014cfe4f6780c653a6889*h)
d=0x8000000000000000000a67f27b3c06329d3445
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffeffffec93
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
