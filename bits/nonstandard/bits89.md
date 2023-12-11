Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |  1ffffff fffffffe ffffde07 |
| b-value |                          7 |
| n-value |  1ffffff ffffee65 030409b9 |
+---------+----------------------------+
| x-value |  10fffff ffffffff 77ffedf2 |
| y-value |   8cfad2 d1cedb24 fc9b3fce |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |   456555 057dbe6d 7b31600b |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffeffffde07
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x10fffffffffffff77ffedf2,0x8cfad2d1cedb24fc9b3fce)
h=1
E.set_order(0x1ffffffffffee65030409b9*h)
d=0xfffffffffff732818204dd
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffeffffde07
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
