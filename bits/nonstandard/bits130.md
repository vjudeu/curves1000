Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |        3 ffffffff ffffffff fffffffe ffffff5f |
| b-value |                                            3 |
| n-value |        4 00000000 00000003 b4cfc1c4 a6929ebf |
+---------+----------------------------------------------+
| x-value |        2 3fffffff ffffffff ffffffff 6fffffa4 |
| y-value |        1 4fffffff ffffffff ffffffff abffffcb |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffeffffff5f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23fffffffffffffffffffffff6fffffa4,0x14fffffffffffffffffffffffabffffcb)
h=1
E.set_order(0x40000000000000003b4cfc1c4a6929ebf*h)
d=0x20000000000000001da67e0e253494f60
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffeffffff5f
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
