Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |        1 ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffe8e43 |
| b-value |                                                                       5 |
| n-value |        1 ffffffff ffffffff ffffffff fffd6a88 6cd16057 307fa863 02915697 |
+---------+-------------------------------------------------------------------------+
| x-value |        1 bfffffff ffffffff ffffffff ffffffff ffffffff ffffffff 1ffebc79 |
| y-value |          1f9ef32e a0925a9f 18fa10b0 3284de69 6283df5f f1495786 28cc9af1 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |          e9d48261 159c0fa5 88cb725c 844eed59 0ff4bc00 2acd8d04 b7da693f |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffefffe8e43
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1bfffffffffffffffffffffffffffffffffffffffffffffff1ffebc79,0x1f9ef32ea0925a9f18fa10b03284de696283df5ff149578628cc9af1)
h=1
E.set_order(0x1fffffffffffffffffffffffffffd6a886cd16057307fa86302915697*h)
d=0xfffffffffffffffffffffffffffeb5443668b02b983fd4318148ab4c
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffefffe8e43
modulo_root=(p+1)/4
x=0
b_value=5
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
