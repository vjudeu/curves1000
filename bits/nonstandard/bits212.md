Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    fffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffa25 |
| b-value |                                                              7 |
| n-value |    fffff ffffffff ffffffff fffffa40 a16e9ea2 5b64dade c59ab071 |
+---------+----------------------------------------------------------------+
| x-value |    99999 99999999 99999999 99999999 99999999 99999998 fffffc7b |
| y-value |    cb5ec 6c37bad1 d4c1e8bd afab20aa b08df9b5 a0d32de4 86044430 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              2 |
| (G/2).y |    42919 530a4b7f cc4456c6 60b378c9 bfce4ecb 563d0b37 7666d291 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffefffffa25
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x999999999999999999999999999999999999999999998fffffc7b,0xcb5ec6c37bad1d4c1e8bdafab20aab08df9b5a0d32de486044430)
h=1
E.set_order(0xffffffffffffffffffffffffffa40a16e9ea25b64dadec59ab071*h)
d=0x7fffffffffffffffffffffffffd2050b74f512db26d6f62cd5839
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffefffffa25
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
