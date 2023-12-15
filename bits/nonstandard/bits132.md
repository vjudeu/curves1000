Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |        f ffffffff ffffffff fffffffe ffffe3ab |
| b-value |                                            5 |
| n-value |        f ffffffff fffffff8 6e7744e9 6b706ced |
+---------+----------------------------------------------+
| x-value |        4 ffffffff ffffffff ffffffff affff728 |
| y-value |          3fffffff ffffffff ffffffff fbffff8a |
+---------+----------------------------------------------+
| (G/2).x |        f ffffffff ffffffff fffffffe ffffe3aa |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffeffffe3ab
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x4ffffffffffffffffffffffffaffff728,0x3ffffffffffffffffffffffffbffff8a)
h=1
E.set_order(0xffffffffffffffff86e7744e96b706ced*h)
d=0x7fffffffffffffffc373ba274b5b83677
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffeffffe3ab
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
