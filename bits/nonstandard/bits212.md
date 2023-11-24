Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |    fffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffa25 |
| b-value |                                                              7 |
| n-value |    fffff ffffffff ffffffff fffffa40 a16e9ea2 5b64dade c59ab071 |
+---------+----------------------------------------------------------------+
| x-value |    99999 99999999 99999999 99999999 99999999 99999998 fffffc7b |
| y-value |    34a13 93c8452e 2b3e1742 5054df55 4f72064a 5f2cd21a 79fbb5f5 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              2 |
| (G/2).y |    bd6e6 acf5b480 33bba939 9f4c8736 4031b134 a9c2f4c7 89992794 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffefffffa25
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x999999999999999999999999999999999999999999998fffffc7b,0x34a1393c8452e2b3e17425054df554f72064a5f2cd21a79fbb5f5)
h=1
E.set_order(0xffffffffffffffffffffffffffa40a16e9ea25b64dadec59ab071*h)
d=0x7fffffffffffffffffffffffffd2050b74f512db26d6f62cd5839
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffefffffa25
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
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
