Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |       7f ffffffff ffffffff ffffffff fffffffe ffffbb9f |
| b-value |                                                     5 |
| n-value |       7f ffffffff ffffffff fffc519f e2d2a587 d1b5a223 |
+---------+-------------------------------------------------------+
| x-value |       47 ffffffff ffffffff ffffffff ffffffff 6fffd98c |
| y-value |       69 ffffffff ffffffff ffffffff ffffffff 2bffc75b |
+---------+-------------------------------------------------------+
| (G/2).x |       7f ffffffff ffffffff ffffffff fffffffe ffffbb9e |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffeffffbb9f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x47ffffffffffffffffffffffffffffffff6fffd98c,0x69ffffffffffffffffffffffffffffffff2bffc75b)
h=1
E.set_order(0x7ffffffffffffffffffffc519fe2d2a587d1b5a223*h)
d=0x3ffffffffffffffffffffe28cff16952c3e8dad112
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffeffffbb9f
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
