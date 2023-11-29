Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |   3fffff ffffffff ffffffff ffffffff fffffffe fffffe1b |
| b-value |                                                     3 |
| n-value |   3fffff ffffffff ffffffff f492bf70 8981c971 8111dc09 |
+---------+-------------------------------------------------------+
| x-value |   13ffff ffffffff ffffffff ffffffff ffffffff afffff67 |
| y-value |   10ffff ffffffff ffffffff ffffffff ffffffff bbffff7f |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffefffffe1b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x13ffffffffffffffffffffffffffffffffffffafffff67,0x10ffffffffffffffffffffffffffffffffffffbbffff7f)
h=1
E.set_order(0x3ffffffffffffffffffffff492bf708981c9718111dc09*h)
d=0x1ffffffffffffffffffffffa495fb844c0e4b8c088ee05
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffefffffe1b
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
