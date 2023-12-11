Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |    1ffff fffffffe ffffed8b |
| b-value |                          7 |
| n-value |    20000 000000e9 8ffa8d81 |
+---------+----------------------------+
| x-value |    1e5d1 745d1744 de8b9166 |
| y-value |      8fa 8f7dbe24 84edb7ed |
+---------+----------------------------+
| (G/2).x |                          5 |
| (G/2).y |     2ed2 aa0061f4 1ded6a03 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffeffffed8b
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x1e5d1745d1744de8b9166,0x8fa8f7dbe2484edb7ed)
h=1
E.set_order(0x20000000000e98ffa8d81*h)
d=0x1000000000074c7fd46c1
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffeffffed8b
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
