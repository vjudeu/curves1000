Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value | 7fffffff ffffffff fffffffe fffff81d |
| b-value |                                   5 |
| n-value | 80000000 00000000 9bfada85 0418ef8f |
+---------+-------------------------------------+
| x-value | 17ffffff ffffffff ffffffff cffffe88 |
| y-value | 4dffffff ffffffff ffffffff 63fffb2d |
+---------+-------------------------------------+
| (G/2).x | 7fffffff ffffffff fffffffe fffff81c |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffefffff81d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x17ffffffffffffffffffffffcffffe88,0x4dffffffffffffffffffffff63fffb2d)
h=1
E.set_order(0x80000000000000009bfada850418ef8f*h)
d=0x40000000000000004dfd6d42820c77c8
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffefffff81d
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=p
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
        x-=1
```
