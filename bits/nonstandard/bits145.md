Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |    1ffff ffffffff ffffffff fffffffe fffff92b |
| b-value |                                            3 |
| n-value |    20000 00000000 000001df 3ea1186d 8bc13eb7 |
+---------+----------------------------------------------+
| x-value |     9fff ffffffff ffffffff ffffffff affffddc |
| y-value |    107ff ffffffff ffffffff ffffffff 7bfffc7a |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffefffff92b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x9fffffffffffffffffffffffffffaffffddc,0x107ffffffffffffffffffffffffff7bfffc7a)
h=1
E.set_order(0x2000000000000000001df3ea1186d8bc13eb7*h)
d=0x1000000000000000000ef9f508c36c5e09f5c
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffefffff92b
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
