Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |   ffffff ffffffff ffffffff ffffffff fffffffe ffffa505 |
| b-value |                                                     7 |
| n-value |  1000000 00000000 00000000 12fb98f7 92d49c91 6b22f293 |
+---------+-------------------------------------------------------+
| x-value |   666666 66666666 66666666 66666666 66666665 ffffdb9a |
| y-value |   dfc5ca d54968d8 a5e6da85 531635fa 8fac7d65 382c1e25 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |   42646f e4be1cd5 1bf56500 7703ef0a e970bfe7 c57c277a |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffeffffa505
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x66666666666666666666666666666666666665ffffdb9a,0xdfc5cad54968d8a5e6da85531635fa8fac7d65382c1e25)
h=1
E.set_order(0x1000000000000000000000012fb98f792d49c916b22f293*h)
d=0x8000000000000000000000097dcc7bc96a4e48b591794a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffeffffa505
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
