Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |   3fffff ffffffff fffffffe fffffa7f |
| b-value |                                   3 |
| n-value |   3fffff ffffffff f4149761 5d93ac91 |
+---------+-------------------------------------+
| x-value |   23ffff ffffffff ffffffff 6ffffce6 |
| y-value |   34ffff ffffffff ffffffff 2bfffb71 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffefffffa7f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23ffffffffffffffffffff6ffffce6,0x34ffffffffffffffffffff2bfffb71)
h=1
E.set_order(0x3ffffffffffffff41497615d93ac91*h)
d=0x1ffffffffffffffa0a4bb0aec9d649
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffefffffa7f
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
