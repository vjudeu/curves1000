Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |   7fffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdb5b |
| b-value |                                                              3 |
| n-value |   800000 00000000 00000000 00001696 498abda8 64f5d438 0754fb7d |
+---------+----------------------------------------------------------------+
| x-value |   27ffff ffffffff ffffffff ffffffff ffffffff ffffffff affff48b |
| y-value |   21ffff ffffffff ffffffff ffffffff ffffffff ffffffff bbfff644 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffeffffdb5b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x27ffffffffffffffffffffffffffffffffffffffffffffaffff48b,0x21ffffffffffffffffffffffffffffffffffffffffffffbbfff644)
h=1
E.set_order(0x800000000000000000000000001696498abda864f5d4380754fb7d*h)
d=0x400000000000000000000000000b4b24c55ed4327aea1c03aa7dbf
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffeffffdb5b
modulo_root=(p+1)/4
x=0
b_value=3
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
