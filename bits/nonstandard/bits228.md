Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |        f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffd3a3 |
| b-value |                                                                       7 |
| n-value |        f ffffffff ffffffff ffffffff fff8cf1f 16cbc563 366d1adf 7a2d54b3 |
+---------+-------------------------------------------------------------------------+
| x-value |        3 33333333 33333333 33333333 33333333 33333333 33333332 fffff71f |
| y-value |        2 f963d9ff 9f35a8f6 04749f36 6f2d263e d60c5b64 4b86aa61 f8cd6e91 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |        3 d9b463d0 9862ae8c 38abf57b b6daf9d4 a0a7b86f 94609a81 c118bd89 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffeffffd3a3
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x3333333333333333333333333333333333333333333333332fffff71f,0x2f963d9ff9f35a8f604749f366f2d263ed60c5b644b86aa61f8cd6e91)
h=1
E.set_order(0xffffffffffffffffffffffffffff8cf1f16cbc563366d1adf7a2d54b3*h)
d=0x7fffffffffffffffffffffffffffc678f8b65e2b19b368d6fbd16aa5a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffeffffd3a3
modulo_root=(p+1)/4
x=0
b_value=7
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
