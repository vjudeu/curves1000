Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |     3fff ffffffff ffffffff fffffffe ffffd9af |
| b-value |                                            3 |
| n-value |     4000 00000000 0000001a 82195197 9f279e23 |
+---------+----------------------------------------------+
| x-value |     23ff ffffffff ffffffff ffffffff 6fffea71 |
| y-value |     24ff ffffffff ffffffff ffffffff 6bffe9d9 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffeffffd9af
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23ffffffffffffffffffffffffff6fffea71,0x24ffffffffffffffffffffffffff6bffe9d9)
h=1
E.set_order(0x4000000000000000001a821951979f279e23*h)
d=0x2000000000000000000d410ca8cbcf93cf12
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffeffffd9af
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
