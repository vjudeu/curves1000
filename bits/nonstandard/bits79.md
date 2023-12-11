Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |     7fff fffffffe ffffe67d |
| b-value |                          7 |
| n-value |     8000 00000005 49394e6b |
+---------+----------------------------+
| x-value |     4ccc cccccccc 333323e3 |
| y-value |     3154 82286db7 6cc701c8 |
+---------+----------------------------+
| (G/2).x |                          2 |
| (G/2).y |     35ee 9c2d5672 c8ad0fa5 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffeffffe67d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x4ccccccccccc333323e3,0x315482286db76cc701c8)
h=1
E.set_order(0x80000000000549394e6b*h)
d=0x400000000002a49ca736
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffeffffe67d
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
