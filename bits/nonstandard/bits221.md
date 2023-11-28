Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value | 1fffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffe9f5 |
| b-value |                                                              5 |
| n-value | 20000000 00000000 00000000 0000b504 968b307b 32aebfd1 43fb2c51 |
+---------+----------------------------------------------------------------+
| x-value | 16276276 27627627 62762762 76276276 27627627 62762761 c4ec3f81 |
| y-value |  6b9fac9 0d610019 441addfb 72823531 83fca638 ff1c8005 44429265 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              2 |
| (G/2).y |  5fc393c d762ced9 ffe05809 3c6ee7ae d84f89f8 adff6d89 6c24df33 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffeffffe9f5
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x162762762762762762762762762762762762762762762761c4ec3f81,0x6b9fac90d610019441addfb7282353183fca638ff1c800544429265)
h=1
E.set_order(0x2000000000000000000000000000b504968b307b32aebfd143fb2c51*h)
d=0x10000000000000000000000000005a824b45983d99575fe8a1fd9629
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffeffffe9f5
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
