Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |        3 ffffffff ffffffff ffffffff ffffffff fffffffe ffffc08f |
| b-value |                                                              3 |
| n-value |        4 00000000 00000000 00000002 2eb2c6b1 a9740837 8640525b |
+---------+----------------------------------------------------------------+
| x-value |        2 3fffffff ffffffff ffffffff ffffffff ffffffff 6fffdc4f |
| y-value |          4fffffff ffffffff ffffffff ffffffff ffffffff ebfffb0b |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffeffffc08f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23fffffffffffffffffffffffffffffffffffffff6fffdc4f,0x4fffffffffffffffffffffffffffffffffffffffebfffb0b)
h=1
E.set_order(0x40000000000000000000000022eb2c6b1a97408378640525b*h)
d=0x200000000000000000000000117596358d4ba041bc320292e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffeffffc08f
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
