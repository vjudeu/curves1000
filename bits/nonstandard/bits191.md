Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value | 7fffffff ffffffff ffffffff ffffffff fffffffe ffff774d |
| b-value |                                                     7 |
| n-value | 7fffffff ffffffff fffffffe ddfd07ea 1a6e97d4 3b966459 |
+---------+-------------------------------------------------------+
| x-value | 78c9ea5d bf193d4b b7e327a9 76fc64f5 2edf8c9d b4479bd9 |
| y-value | 7d014f66 5c05afc8 067741dd 06ce9473 829ed57b 6c6f68b1 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     4 |
| (G/2).y | 2ee71017 0fb1b98d 77b93913 c836a2e2 7ef70d12 549f97ca |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffeffff774d
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x78c9ea5dbf193d4bb7e327a976fc64f52edf8c9db4479bd9,0x7d014f665c05afc8067741dd06ce9473829ed57b6c6f68b1)
h=1
E.set_order(0x7ffffffffffffffffffffffeddfd07ea1a6e97d43b966459*h)
d=0x3fffffffffffffffffffffff6efe83f50d374bea1dcb322d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffeffff774d
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
