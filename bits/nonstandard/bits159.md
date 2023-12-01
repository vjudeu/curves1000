Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value | 7fffffff ffffffff ffffffff fffffffe fffff127 |
| b-value |                                            3 |
| n-value | 80000000 00000000 00002524 623e651b 6a91d46f |
+---------+----------------------------------------------+
| x-value |  7ffffff ffffffff ffffffff ffffffff efffff11 |
| y-value | 79ffffff ffffffff ffffffff ffffffff 0bfff1d9 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffefffff127
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x7ffffffffffffffffffffffffffffffefffff11,0x79ffffffffffffffffffffffffffffff0bfff1d9)
h=1
E.set_order(0x800000000000000000002524623e651b6a91d46f*h)
d=0x400000000000000000001292311f328db548ea38
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffefffff127
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
