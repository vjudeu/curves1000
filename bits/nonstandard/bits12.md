Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------+
| p-value |      fa3 |
| b-value |        5 |
| n-value |     100f |
+---------+----------+
| x-value |      dad |
| y-value |      b03 |
+---------+----------+
| (G/2).x |        1 |
| (G/2).y |      381 |
+---------+----------+
```
Sage code for testing half of the generator:
```
p=0xfa3
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xdad,0xb03)
h=1
E.set_order(0x100f*h)
d=0x808
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfa3
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