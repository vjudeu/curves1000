Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------+
| p-value |  7fffffe fffffbdd |
| b-value |                 5 |
| n-value |  7fffffe accfb2a1 |
+---------+-------------------+
| x-value |  1bd37a6 bd37a60f |
| y-value |  7ab57fc 77769b6e |
+---------+-------------------+
| (G/2).x |                 4 |
| (G/2).y |  307474a 3c9dc073 |
+---------+-------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffefffffbdd
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1bd37a6bd37a60f,0x7ab57fc77769b6e)
h=1
E.set_order(0x7fffffeaccfb2a1*h)
d=0x3ffffff5667d951
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffefffffbdd
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
