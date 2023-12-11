Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |     3ffe fffff9c5 |
| b-value |                 7 |
| n-value |     3fff 00febbfd |
+---------+-------------------+
| x-value |     2327 1cd8531d |
| y-value |      e2e eb0ca4c4 |
+---------+-------------------+
| (G/2).x |                 4 |
| (G/2).y |     1f04 3ed1e932 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffefffff9c5
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x23271cd8531d,0xe2eeb0ca4c4)
h=1
E.set_order(0x3fff00febbfd*h)
d=0x1fff807f5dff
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffefffff9c5
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=0
b_value=7
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    first_y=y_square.powermod(first_modulo_root,p)
    is_on_curve=(first_y.powermod(2,p)==y_square)
    y=first_y
    if not is_on_curve:
        second_y=(first_y*second_multiplier)%p
        is_on_curve=(second_y.powermod(2,p)==y_square)
        y=second_y
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
