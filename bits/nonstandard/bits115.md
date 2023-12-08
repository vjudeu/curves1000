Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |    7ffff ffffffff fffffffe fffff77b |
| b-value |                                   7 |
| n-value |    80000 00000000 02357788 444c86a7 |
+---------+-------------------------------------+
| x-value |     9039 b0ad1207 3615a240 d4bb7d99 |
| y-value |    25394 269255ab 8a0ff7ed 806bb69b |
+---------+-------------------------------------+
| (G/2).x |                                   4 |
| (G/2).y |    1b013 7433071c 1d5ff868 d1e02b69 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffefffff77b
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x9039b0ad12073615a240d4bb7d99,0x25394269255ab8a0ff7ed806bb69b)
h=1
E.set_order(0x800000000000002357788444c86a7*h)
d=0x4000000000000011abbc422264354
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffefffff77b
modulo_root=(p+1)/4
x=0
b_value=7
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
