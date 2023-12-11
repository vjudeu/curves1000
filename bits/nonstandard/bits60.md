Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |  ffffffe fffffe9f |
| b-value |                 5 |
| n-value |  fffffff 70ff7635 |
+---------+-------------------+
| x-value |  13b13b1 2762760b |
| y-value |  ee193d5 2dbc07c5 |
+---------+-------------------+
| (G/2).x |                 2 |
| (G/2).y |   3a5832 66600613 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffefffffe9f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x13b13b12762760b,0xee193d52dbc07c5)
h=1
E.set_order(0xfffffff70ff7635*h)
d=0x7ffffffb87fbb1b
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffefffffe9f
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
