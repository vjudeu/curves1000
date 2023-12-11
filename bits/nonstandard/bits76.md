Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |      fff fffffffe ffffe375 |
| b-value |                          5 |
| n-value |      fff ffffff7f 160f6e03 |
+---------+----------------------------+
| x-value |      f3b 4aa339e4 8a8ccc6c |
| y-value |      2fe 8b48f7bb ef31a19d |
+---------+----------------------------+
| (G/2).x |                          9 |
| (G/2).y |      7f7 d5691d1b b61be958 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffeffffe375
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xf3b4aa339e48a8ccc6c,0x2fe8b48f7bbef31a19d)
h=1
E.set_order(0xfffffffff7f160f6e03*h)
d=0x7ffffffffbf8b07b702
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffeffffe375
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=0
b_value=5
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
