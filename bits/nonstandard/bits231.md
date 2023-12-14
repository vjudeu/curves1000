Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |       7f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffebe7 |
| b-value |                                                                       5 |
| n-value |       80 00000000 00000000 00000000 000a4493 0f71ae49 01a983ef 0b154215 |
+---------+-------------------------------------------------------------------------+
| x-value |        7 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff effffec1 |
| y-value |       39 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 8bfff6e0 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |       7f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffebe6 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffeffffebe7
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x7ffffffffffffffffffffffffffffffffffffffffffffffffeffffec1,0x39ffffffffffffffffffffffffffffffffffffffffffffffff8bfff6e0)
h=1
E.set_order(0x80000000000000000000000000000a44930f71ae4901a983ef0b154215*h)
d=0x400000000000000000000000000005224987b8d72480d4c1f7858aa10b
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffeffffebe7
modulo_root=(p+1)/4
x=p
b_value=5
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
        x-=1
```
