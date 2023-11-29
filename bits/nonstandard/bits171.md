Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |      7ff ffffffff ffffffff ffffffff fffffffe ffffa69f |
| b-value |                                                     3 |
| n-value |      800 00000000 00000000 0055f987 c4ab2e5f 11d9633f |
+---------+-------------------------------------------------------+
| x-value |      47f ffffffff ffffffff ffffffff ffffffff 6fffcdb8 |
| y-value |      29f ffffffff ffffffff ffffffff ffffffff abffe2ac |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffeffffa69f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x47fffffffffffffffffffffffffffffffff6fffcdb8,0x29fffffffffffffffffffffffffffffffffabffe2ac)
h=1
E.set_order(0x80000000000000000000055f987c4ab2e5f11d9633f*h)
d=0x4000000000000000000002afcc3e255972f88ecb1a0
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffeffffa69f
modulo_root=(p+1)/4
x=0
b_value=3
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
