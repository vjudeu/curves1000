Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value | 1ffffffe fffff793 |
| b-value |                 7 |
| n-value | 1ffffffe e235ac2d |
+---------+-------------------+
| x-value | 1deb851d c8f5bab0 |
| y-value |  eba88dd 8944a020 |
+---------+-------------------+
| (G/2).x |                 7 |
| (G/2).y |  addd90b 6271927c |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffefffff793
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x1deb851dc8f5bab0,0xeba88dd8944a020)
h=1
E.set_order(0x1ffffffee235ac2d*h)
d=0xfffffff711ad617
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffefffff793
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
