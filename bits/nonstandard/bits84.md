Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |    fffff fffffffe ffffff3b |
| b-value |                          5 |
| n-value |   100000 000007fc 3e431259 |
+---------+----------------------------+
| x-value |    4ffff ffffffff afffffc5 |
| y-value |    43fff ffffffff bbffffc7 |
+---------+----------------------------+
| (G/2).x |    fffff fffffffe ffffff3a |
| (G/2).y |                          2 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffeffffff3b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x4ffffffffffffafffffc5,0x43fffffffffffbbffffc7)
h=1
E.set_order(0x100000000007fc3e431259*h)
d=0x80000000003fe1f21892d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffeffffff3b
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
