Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |     1fff fffffffe fffffa3f |
| b-value |                          3 |
| n-value |     2000 000000af 1078c7b3 |
+---------+----------------------------+
| x-value |     11ff ffffffff 6ffffcc2 |
| y-value |     1a7f ffffffff 2bfffb3c |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |                          2 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffefffffa3f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x11ffffffffff6ffffcc2,0x1a7fffffffff2bfffb3c)
h=1
E.set_order(0x2000000000af1078c7b3*h)
d=0x100000000057883c63da
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffefffffa3f
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
