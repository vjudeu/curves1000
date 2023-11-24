Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    1ffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdced |
| b-value |                                                              5 |
| n-value |    20000 00000000 00000000 00000246 f6bf684a 97ecd7a7 7b2d5e75 |
+---------+----------------------------------------------------------------+
| x-value |     7627 62762762 76276276 27627627 62762762 76276275 ec4ebcd3 |
| y-value |     14f2 7c35bb60 d7961737 e001c3bb 61f65dd5 df1f9e52 174e1be2 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              2 |
| (G/2).y |    1bb9d 8ac9efa6 5759b3e6 f975b6a2 8cdcf694 a14cb790 c695ae45 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffeffffdced
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x76276276276276276276276276276276276276276275ec4ebcd3,0x14f27c35bb60d7961737e001c3bb61f65dd5df1f9e52174e1be2)
h=1
E.set_order(0x20000000000000000000000000246f6bf684a97ecd7a77b2d5e75*h)
d=0x100000000000000000000000001237b5fb4254bf66bd3bd96af3b
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffeffffdced
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
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
