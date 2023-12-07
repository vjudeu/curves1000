Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value | 7fffffff ffffffff fffffffe fffff81d |
| b-value |                                   5 |
| n-value | 80000000 00000000 9bfada85 0418ef8f |
+---------+-------------------------------------+
| x-value | 2e55b82e 55b82e55 b82e55b7 d1aa44f8 |
| y-value | 1903e20b 3003c753 57fbd425 431082f2 |
+---------+-------------------------------------+
| (G/2).x |                                   6 |
| (G/2).y | 1a608848 b78d315c f2f07283 7d64f6d6 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffefffff81d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x2e55b82e55b82e55b82e55b7d1aa44f8,0x1903e20b3003c75357fbd425431082f2)
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
