Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |     ffff fffffffe fffffcc5 |
| b-value |                          5 |
| n-value |     ffff fffffecc d3b2533d |
+---------+----------------------------+
| x-value |      81b cd081bcc ffffffe7 |
| y-value |     a6fe f58fef90 0144cbdb |
+---------+----------------------------+
| (G/2).x |                          6 |
| (G/2).y |     6f4b 3716c652 f76ce5ab |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffefffffcc5
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x81bcd081bccffffffe7,0xa6fef58fef900144cbdb)
h=1
E.set_order(0xfffffffffeccd3b2533d*h)
d=0x7fffffffff6669d9299f
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffefffffcc5
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
