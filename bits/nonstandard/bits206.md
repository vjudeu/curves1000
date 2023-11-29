Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |     3fff ffffffff ffffffff ffffffff ffffffff fffffffe ffff2f45 |
| b-value |                                                              7 |
| n-value |     4000 00000000 00000000 000000e4 51df05fb 3de46ebd d2f20929 |
+---------+----------------------------------------------------------------+
| x-value |     2675 181b8d33 a8c0dc69 9d4606e3 4cea3037 1a675181 1efe5cb2 |
| y-value |     18e4 d96cdc3e 7f6caf0c 0730ec73 bbabeaf9 dcd98c70 cfb3abd5 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              6 |
| (G/2).y |      b00 95e732ec 7f119f07 225eaca8 95fd579e 345c2fb7 0e97b5d2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffeffff2f45
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x2675181b8d33a8c0dc699d4606e34cea30371a6751811efe5cb2,0x18e4d96cdc3e7f6caf0c0730ec73bbabeaf9dcd98c70cfb3abd5)
h=1
E.set_order(0x40000000000000000000000000e451df05fb3de46ebdd2f20929*h)
d=0x200000000000000000000000007228ef82fd9ef2375ee9790495
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffeffff2f45
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
