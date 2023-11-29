Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |       1f ffffffff ffffffff ffffffff fffffffe ffffa693 |
| b-value |                                                     3 |
| n-value |       20 00000000 00000000 0008a5f3 e47f957c 305024f9 |
+---------+-------------------------------------------------------+
| x-value |       19 ffffffff ffffffff ffffffff ffffffff 2fffb756 |
| y-value |       14 7fffffff ffffffff ffffffff ffffffff 5bffc6b6 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |                                                     2 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffeffffa693
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x19ffffffffffffffffffffffffffffffff2fffb756,0x147fffffffffffffffffffffffffffffff5bffc6b6)
h=1
E.set_order(0x2000000000000000000008a5f3e47f957c305024f9*h)
d=0x100000000000000000000452f9f23fcabe1828127d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffeffffa693
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
