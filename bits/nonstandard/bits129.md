Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |        1 ffffffff ffffffff fffffffe fffff4cf |
| b-value |                                            7 |
| n-value |        1 ffffffff fffffffd 6f5140ac 1bea9a4d |
+---------+----------------------------------------------+
| x-value |        1 8fffffff ffffffff ffffffff 37fff740 |
| y-value |        1 50e0c360 66e897f3 f5c9e9f1 982c6bba |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |          22ed1ac5 0b67d74c 934cab74 b5310e5a |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffefffff4cf
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x18fffffffffffffffffffffff37fff740,0x150e0c36066e897f3f5c9e9f1982c6bba)
h=1
E.set_order(0x1fffffffffffffffd6f5140ac1bea9a4d*h)
d=0xfffffffffffffffeb7a8a0560df54d27
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffefffff4cf
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
