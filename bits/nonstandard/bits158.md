Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff fffffffe fffff11f |
| b-value |                                            3 |
| n-value | 40000000 00000000 00000b36 0d0b25ec c9900523 |
+---------+----------------------------------------------+
| x-value | 23ffffff ffffffff ffffffff ffffffff 6ffff7a0 |
| y-value | 14ffffff ffffffff ffffffff ffffffff abfffb1e |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffefffff11f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23ffffffffffffffffffffffffffffff6ffff7a0,0x14ffffffffffffffffffffffffffffffabfffb1e)
h=1
E.set_order(0x400000000000000000000b360d0b25ecc9900523*h)
d=0x20000000000000000000059b068592f664c80292
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffefffff11f
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
