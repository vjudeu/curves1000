Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |        f ffffffff ffffffff ffffffff ffffffff fffffffe ffffce27 |
| b-value |                                                              3 |
| n-value |        f ffffffff ffffffff fffffff8 02bae8f3 c5912172 8b13b199 |
+---------+----------------------------------------------------------------+
| x-value |          ffffffff ffffffff ffffffff ffffffff ffffffff effffce1 |
| y-value |        f 3fffffff ffffffff ffffffff ffffffff ffffffff 0bffd07d |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffeffffce27
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xffffffffffffffffffffffffffffffffffffffffeffffce1,0xf3fffffffffffffffffffffffffffffffffffffff0bffd07d)
h=1
E.set_order(0xffffffffffffffffffffffff802bae8f3c59121728b13b199*h)
d=0x7fffffffffffffffffffffffc015d7479e2c890b94589d8cd
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffeffffce27
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
