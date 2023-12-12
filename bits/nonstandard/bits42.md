Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |      3fe fffff79d |
| b-value |                 7 |
| n-value |      3ff 003096f7 |
+---------+-------------------+
| x-value |      265 ccccc7c3 |
| y-value |      37a 6be79c2a |
+---------+-------------------+
| (G/2).x |                 2 |
| (G/2).y |      1ba af6b8727 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x3fefffff79d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x265ccccc7c3,0x37a6be79c2a)
h=1
E.set_order(0x3ff003096f7*h)
d=0x1ff80184b7c
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fefffff79d
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
