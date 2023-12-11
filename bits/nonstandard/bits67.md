Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |        7 fffffffe fffff43f |
| b-value |                          3 |
| n-value |        7 fffffffc 983ce347 |
+---------+----------------------------+
| x-value |        4 7fffffff 6ffff962 |
| y-value |        6 9fffffff 2bfff644 |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |                          2 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffefffff43f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x47fffffff6ffff962,0x69fffffff2bfff644)
h=1
E.set_order(0x7fffffffc983ce347*h)
d=0x3fffffffe4c1e71a4
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffefffff43f
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
