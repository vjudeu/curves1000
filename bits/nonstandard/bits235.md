Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |      7ff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffa84f |
| b-value |                                                                       3 |
| n-value |      7ff ffffffff ffffffff ffffffff fffedd64 f71a76d1 2529ed9b 30c7cffd |
+---------+-------------------------------------------------------------------------+
| x-value |      47f ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6fffceab |
| y-value |       9f ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff ebfff926 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffeffffa84f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x47fffffffffffffffffffffffffffffffffffffffffffffffff6fffceab,0x9fffffffffffffffffffffffffffffffffffffffffffffffffebfff926)
h=1
E.set_order(0x7fffffffffffffffffffffffffffffedd64f71a76d12529ed9b30c7cffd*h)
d=0x3ffffffffffffffffffffffffffffff6eb27b8d3b689294f6cd9863e7ff
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffffffeffffa84f
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
