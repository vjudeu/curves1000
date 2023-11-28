Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |   3fffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffa145 |
| b-value |                                                              5 |
| n-value |   3fffff ffffffff ffffffff fffff002 54af334d 2aa08a7d 0267a025 |
+---------+----------------------------------------------------------------+
| x-value |   3a3548 fa3548fa 3548fa35 48fa3548 fa3548fa 3548fa34 6024bb39 |
| y-value |   171de1 4b8cadc0 b82cb066 7dc5aa5e 24c37c80 6fa97834 603d3b6c |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              6 |
| (G/2).y |    7a933 c9ea39a4 3f3f9626 836de36e 5398b7cf 3db43451 0dff0b13 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffeffffa145
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x3a3548fa3548fa3548fa3548fa3548fa3548fa3548fa346024bb39,0x171de14b8cadc0b82cb0667dc5aa5e24c37c806fa97834603d3b6c)
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
