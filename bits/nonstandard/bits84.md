Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |    fffff fffffffe ffffff3b |
| b-value |                          5 |
| n-value |   100000 000007fc 3e431259 |
+---------+----------------------------+
| x-value |    dffff ffffffff 1fffff52 |
| y-value |    55e5e 698257b2 fc1ee9f5 |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |    6334a 6ff92f6a 511a6d9f |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffeffffff3b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xdffffffffffff1fffff52,0x55e5e698257b2fc1ee9f5)
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
