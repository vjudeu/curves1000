Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value | 7fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffef53 |
| b-value |                                                                       5 |
| n-value | 7fffffff ffffffff ffffffff ffffffff 00f26097 ca79ff9a bcdac70f f2f55f6d |
+---------+-------------------------------------------------------------------------+
| x-value | 67ffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2ffff276 |
| y-value | 11ffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff dbfffda3 |
+---------+-------------------------------------------------------------------------+
| (G/2).x | 7fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffef52 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffffffeffffef53
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x67ffffffffffffffffffffffffffffffffffffffffffffffffffffff2ffff276,0x11ffffffffffffffffffffffffffffffffffffffffffffffffffffffdbfffda3)
h=1
E.set_order(0x7fffffffffffffffffffffffffffffff00f26097ca79ff9abcdac70ff2f55f6d*h)
d=0x3fffffffffffffffffffffffffffffff8079304be53cffcd5e6d6387f97aafb7
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffffffffeffffef53
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
