Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |  3ffffff ffffffff ffffffff fffffffe fffffbdb |
| b-value |                                            5 |
| n-value |  3ffffff ffffffff ffffea7f 484ddbcc 4039b86b |
+---------+----------------------------------------------+
| x-value |  37fffff ffffffff ffffffff ffffffff 1ffffc5e |
| y-value |  3b922db 474cf7da 9fab91fd 353d6a86 cb938948 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |   13f7b0 a4f18c0f a3de574d f0c1b2d2 3b6a083e |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffefffffbdb
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x37fffffffffffffffffffffffffffff1ffffc5e,0x3b922db474cf7da9fab91fd353d6a86cb938948)
h=1
E.set_order(0x3ffffffffffffffffffea7f484ddbcc4039b86b*h)
d=0x1fffffffffffffffffff53fa426ede6201cdc36
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffefffffbdb
modulo_root=(p+1)/4
x=0
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
        x+=1
```
