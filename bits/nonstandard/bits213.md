Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |   1fffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffd69f |
| b-value |                                                              3 |
| n-value |   1fffff ffffffff ffffffff fffffbb4 c27e2215 b4f97b02 5543c01b |
+---------+----------------------------------------------------------------+
| x-value |   11ffff ffffffff ffffffff ffffffff ffffffff ffffffff 6fffe8b8 |
| y-value |    a7fff ffffffff ffffffff ffffffff ffffffff ffffffff abfff26c |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffeffffd69f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x11ffffffffffffffffffffffffffffffffffffffffffff6fffe8b8,0xa7fffffffffffffffffffffffffffffffffffffffffffabfff26c)
h=1
E.set_order(0x1ffffffffffffffffffffffffffbb4c27e2215b4f97b025543c01b*h)
d=0xffffffffffffffffffffffffffdda613f110ada7cbd812aa1e00e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffeffffd69f
modulo_root=(p+1)/4
x=0
b_value=3
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
