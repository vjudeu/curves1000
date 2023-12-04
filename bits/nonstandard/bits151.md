Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |   7fffff ffffffff ffffffff fffffffe fffff643 |
| b-value |                                            7 |
| n-value |   800000 00000000 0000093c a0b5d636 3477ae41 |
+---------+----------------------------------------------+
| x-value |    4b4b4 b4b4b4b4 b4b4b4b4 b4b4b4b4 ab4b4aef |
| y-value |    77ce0 e55cf810 f136377f 299e0089 b2fb27ce |
+---------+----------------------------------------------+
| (G/2).x |                                            3 |
| (G/2).y |    22d18 95979127 bc750fa8 8ebb673f 8032ce4f |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffefffff643
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x4b4b4b4b4b4b4b4b4b4b4b4b4b4b4ab4b4aef,0x77ce0e55cf810f136377f299e0089b2fb27ce)
h=1
E.set_order(0x800000000000000000093ca0b5d6363477ae41*h)
d=0x400000000000000000049e505aeb1b1a3bd721
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffefffff643
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
