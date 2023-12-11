Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |  3ffffff fffffffe fffff72b |
| b-value |                          7 |
| n-value |  4000000 0000295a c36ddcef |
+---------+----------------------------+
| x-value |  3333333 33333332 66665f54 |
| y-value |   b66d5b dbed46dd 4ddc8b6d |
+---------+----------------------------+
| (G/2).x |                          2 |
| (G/2).y |   2a3e5c 683289c8 c83b118b |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffefffff72b
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x33333333333333266665f54,0xb66d5bdbed46dd4ddc8b6d)
h=1
E.set_order(0x40000000000295ac36ddcef*h)
d=0x2000000000014ad61b6ee78
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffefffff72b
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
