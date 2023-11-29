Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |  7ffffff ffffffff ffffffff ffffffff fffffffe ffffbe57 |
| b-value |                                                     3 |
| n-value |  7ffffff ffffffff ffffffff fa6a7eac 2b38a87e 917d538b |
+---------+-------------------------------------------------------+
| x-value |   7fffff ffffffff ffffffff ffffffff ffffffff effffbe4 |
| y-value |  59fffff ffffffff ffffffff ffffffff ffffffff 4bffd1d5 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffeffffbe57
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x7fffffffffffffffffffffffffffffffffffffeffffbe4,0x59fffffffffffffffffffffffffffffffffffff4bffd1d5)
h=1
E.set_order(0x7fffffffffffffffffffffffa6a7eac2b38a87e917d538b*h)
d=0x3fffffffffffffffffffffffd353f56159c543f48bea9c6
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffeffffbe57
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
