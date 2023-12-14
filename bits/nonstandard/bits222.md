Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdacf |
| b-value |                                                              5 |
| n-value | 3fffffff ffffffff ffffffff ffff1676 af8f9385 db82f0e7 e58c3aa1 |
+---------+----------------------------------------------------------------+
| x-value | 23ffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6fffeb17 |
| y-value | 24ffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6bffea7b |
+---------+----------------------------------------------------------------+
| (G/2).x | 3fffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdace |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffeffffdacf
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x23ffffffffffffffffffffffffffffffffffffffffffffff6fffeb17,0x24ffffffffffffffffffffffffffffffffffffffffffffff6bffea7b)
h=1
E.set_order(0x3fffffffffffffffffffffffffff1676af8f9385db82f0e7e58c3aa1*h)
d=0x1fffffffffffffffffffffffffff8b3b57c7c9c2edc17873f2c61d51
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffeffffdacf
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
