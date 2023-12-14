Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |   3fffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffa145 |
| b-value |                                                              5 |
| n-value |   3fffff ffffffff ffffffff fffff002 54af334d 2aa08a7d 0267a025 |
+---------+----------------------------------------------------------------+
| x-value |   2bffff ffffffff ffffffff ffffffff ffffffff ffffffff 4fffbee2 |
| y-value |   2effff ffffffff ffffffff ffffffff ffffffff ffffffff 43ffba6a |
+---------+----------------------------------------------------------------+
| (G/2).x |   3fffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffa144 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffeffffa145
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x2bffffffffffffffffffffffffffffffffffffffffffff4fffbee2,0x2effffffffffffffffffffffffffffffffffffffffffff43ffba6a)
h=1
E.set_order(0x3ffffffffffffffffffffffffff00254af334d2aa08a7d0267a025*h)
d=0x1ffffffffffffffffffffffffff8012a5799a69550453e8133d013
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffeffffa145
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
