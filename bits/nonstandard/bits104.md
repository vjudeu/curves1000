Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |       ff ffffffff fffffffe ffffad6f |
| b-value |                                   5 |
| n-value |       ff ffffffff ffe15794 a28b92db |
+---------+-------------------------------------+
| x-value |       3b 13b13b13 b13b13b0 ffffecf1 |
| y-value |       51 947e4197 a7c371e0 6065b5b6 |
+---------+-------------------------------------+
| (G/2).x |                                   2 |
| (G/2).y |       77 d7fdfdd8 4315aa2e d3b36527 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffeffffad6f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x3b13b13b13b13b13b0ffffecf1,0x51947e4197a7c371e06065b5b6)
h=1
E.set_order(0xffffffffffffe15794a28b92db*h)
d=0x7ffffffffffff0abca5145c96e
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffeffffad6f
modulo_root=(p+1)/4
x=0
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
        x+=1
```
