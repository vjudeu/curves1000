Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |  3ffffff ffffffff ffffffff ffffffff fffffffe ffff61eb |
| b-value |                                                     3 |
| n-value |  3ffffff ffffffff ffffffff c0508b7d b73050c0 c62c43a5 |
+---------+-------------------------------------------------------+
| x-value |  13fffff ffffffff ffffffff ffffffff ffffffff afffce98 |
| y-value |  20fffff ffffffff ffffffff ffffffff ffffffff 7bffae7d |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffeffff61eb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x13fffffffffffffffffffffffffffffffffffffafffce98,0x20fffffffffffffffffffffffffffffffffffff7bffae7d)
h=1
E.set_order(0x3ffffffffffffffffffffffc0508b7db73050c0c62c43a5*h)
d=0x1ffffffffffffffffffffffe02845bedb982860631621d3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffeffff61eb
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
