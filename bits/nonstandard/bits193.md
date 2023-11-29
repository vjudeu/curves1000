Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |        1 ffffffff ffffffff ffffffff ffffffff fffffffe ffffe7bb |
| b-value |                                                              3 |
| n-value |        2 00000000 00000000 00000001 b7f0f5ab 214ce2d9 f3016997 |
+---------+----------------------------------------------------------------+
| x-value |          9fffffff ffffffff ffffffff ffffffff ffffffff affff869 |
| y-value |        1 87ffffff ffffffff ffffffff ffffffff ffffffff 3bffed6b |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffeffffe7bb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x9fffffffffffffffffffffffffffffffffffffffaffff869,0x187ffffffffffffffffffffffffffffffffffffff3bffed6b)
h=1
E.set_order(0x2000000000000000000000001b7f0f5ab214ce2d9f3016997*h)
d=0x1000000000000000000000000dbf87ad590a6716cf980b4cc
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffeffffe7bb
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
