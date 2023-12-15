Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |       1f ffffffff fffffffe fffff403 |
| b-value |                                   5 |
| n-value |       1f ffffffff fff5c0c0 258167b3 |
+---------+-------------------------------------+
| x-value |       19 ffffffff ffffffff 2ffff645 |
| y-value |       1c 7fffffff ffffffff 1bfff54e |
+---------+-------------------------------------+
| (G/2).x |       1f ffffffff fffffffe fffff402 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffefffff403
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x19ffffffffffffffff2ffff645,0x1c7fffffffffffffff1bfff54e)
h=1
E.set_order(0x1ffffffffffff5c0c0258167b3*h)
d=0xffffffffffffae06012c0b3da
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffefffff403
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
