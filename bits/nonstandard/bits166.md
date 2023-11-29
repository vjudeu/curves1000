Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |       3f ffffffff ffffffff ffffffff fffffffe ffffec87 |
| b-value |                                                     3 |
| n-value |       3f ffffffff ffffffff fff20b50 55bb6439 8ba48161 |
+---------+-------------------------------------------------------+
| x-value |        3 ffffffff ffffffff ffffffff ffffffff effffec7 |
| y-value |       1c ffffffff ffffffff ffffffff ffffffff 8bfff72d |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffeffffec87
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x3ffffffffffffffffffffffffffffffffeffffec7,0x1cffffffffffffffffffffffffffffffff8bfff72d)
h=1
E.set_order(0x3ffffffffffffffffffff20b5055bb64398ba48161*h)
d=0x1ffffffffffffffffffff905a82addb21cc5d240b1
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffeffffec87
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
