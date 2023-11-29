Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |       3f ffffffff ffffffff ffffffff ffffffff fffffffe ffffb72f |
| b-value |                                                              3 |
| n-value |       3f ffffffff ffffffff fffffff2 94c408ac 1f2f97ff 4f747d81 |
+---------+----------------------------------------------------------------+
| x-value |       23 ffffffff ffffffff ffffffff ffffffff ffffffff 6fffd709 |
| y-value |       24 ffffffff ffffffff ffffffff ffffffff ffffffff 6bffd5e7 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffeffffb72f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23ffffffffffffffffffffffffffffffffffffffff6fffd709,0x24ffffffffffffffffffffffffffffffffffffffff6bffd5e7)
h=1
E.set_order(0x3ffffffffffffffffffffffff294c408ac1f2f97ff4f747d81*h)
d=0x1ffffffffffffffffffffffff94a6204560f97cbffa7ba3ec1
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffeffffb72f
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
