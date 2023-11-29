Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |       7f ffffffff ffffffff ffffffff fffffffe ffffbb9f |
| b-value |                                                     5 |
| n-value |       7f ffffffff ffffffff fffc519f e2d2a587 d1b5a223 |
+---------+-------------------------------------------------------+
| x-value |       27 62762762 76276276 27627627 62762762 2762611c |
| y-value |       51 b3418d91 38e5e48e 9cb91f86 c345c200 da0b3954 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |       31 260d2fac 51ecc509 b450d753 2ae77b99 c84e9dc0 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffeffffbb9f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x27627627627627627627627627627627622762611c,0x51b3418d9138e5e48e9cb91f86c345c200da0b3954)
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
