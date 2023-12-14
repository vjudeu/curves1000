Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |       3f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff7df |
| b-value |                                                                       5 |
| n-value |       40 00000000 00000000 00000000 0002202e 64b91c4e cccb3277 dd18b89f |
+---------+-------------------------------------------------------------------------+
| x-value |       23 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6ffffb70 |
| y-value |       34 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2bfff940 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |       3f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff7de |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffefffff7df
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x23ffffffffffffffffffffffffffffffffffffffffffffffff6ffffb70,0x34ffffffffffffffffffffffffffffffffffffffffffffffff2bfff940)
h=1
E.set_order(0x400000000000000000000000000002202e64b91c4ecccb3277dd18b89f*h)
d=0x2000000000000000000000000000011017325c8e276665993bee8c5c50
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffefffff7df
modulo_root=(p+1)/4
x=p
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
        x-=1
```
