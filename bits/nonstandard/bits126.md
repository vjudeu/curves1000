Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value | 3fffffff ffffffff fffffffe ffffd823 |
| b-value |                                   5 |
| n-value | 3fffffff ffffffff ff6551df ef584693 |
+---------+-------------------------------------+
| x-value | 37ffffff ffffffff ffffffff 1fffdd1d |
| y-value | 3429a02f 85d83f5b e06fbfed 23b28dd7 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |  b29d0ea 1d8aea97 a1a3a31d 0c83cf5c |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffeffffd823
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x37ffffffffffffffffffffff1fffdd1d,0x3429a02f85d83f5be06fbfed23b28dd7)
h=1
E.set_order(0x3fffffffffffffffff6551dfef584693*h)
d=0x1fffffffffffffffffb2a8eff7ac234a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffeffffd823
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
