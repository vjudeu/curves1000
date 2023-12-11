Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |   7fffff fffffffe fffff25f |
| b-value |                          3 |
| n-value |   7fffff ffffed63 cbb4f96d |
+---------+----------------------------+
| x-value |   47ffff ffffffff 6ffff854 |
| y-value |   29ffff ffffffff abfffb87 |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |                          2 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffefffff25f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x47ffffffffffff6ffff854,0x29ffffffffffffabfffb87)
h=1
E.set_order(0x7fffffffffed63cbb4f96d*h)
d=0x3ffffffffff6b1e5da7cb7
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffefffff25f
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
