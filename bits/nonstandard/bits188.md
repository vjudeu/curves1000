Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |  fffffff ffffffff ffffffff ffffffff fffffffe ffff9f1d |
| b-value |                                                     5 |
| n-value | 10000000 00000000 00000000 42b72388 4bfa6490 da0e2a87 |
+---------+-------------------------------------------------------+
| x-value |  3b13b13 b13b13b1 3b13b13b 13b13b13 b13b13b0 ffffe9a3 |
| y-value |  9598411 f71a8c1c f25bbd68 a81669cb bbec5413 68ffee82 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |  33ae4d1 4d801409 971d7e94 532a8d64 3ac8862e adb32058 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffeffff9f1d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x3b13b13b13b13b13b13b13b13b13b13b13b13b0ffffe9a3,0x9598411f71a8c1cf25bbd68a81669cbbbec541368ffee82)
h=1
E.set_order(0x10000000000000000000000042b723884bfa6490da0e2a87*h)
d=0x80000000000000000000000215b91c425fd32486d071544
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffeffff9f1d
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
