Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |  fffffff ffffffff ffffffff fffffffe ffffe9ff |
| b-value |                                            3 |
| n-value | 10000000 00000000 00002bba 79b7b506 ae223e1b |
+---------+----------------------------------------------+
| x-value |  8ffffff ffffffff ffffffff ffffffff 6ffff39e |
| y-value |  d3fffff ffffffff ffffffff ffffffff 2bffedc7 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffeffffe9ff
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x8ffffffffffffffffffffffffffffff6ffff39e,0xd3fffffffffffffffffffffffffffff2bffedc7)
h=1
E.set_order(0x100000000000000000002bba79b7b506ae223e1b*h)
d=0x800000000000000000015dd3cdbda8357111f0e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffeffffe9ff
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
