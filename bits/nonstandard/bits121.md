Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |  1ffffff ffffffff fffffffe ffffedc7 |
| b-value |                                   7 |
| n-value |  1ffffff ffffffff f8c1318f f606a089 |
+---------+-------------------------------------+
| x-value |  10fffff ffffffff ffffffff 77fff650 |
| y-value |  169a10d cca26c07 fb0c0166 768cefc1 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |   4c3b0b a0df858b c4bfbbdc d40a372b |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffeffffedc7
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x10fffffffffffffffffffff77fff650,0x169a10dcca26c07fb0c0166768cefc1)
h=1
E.set_order(0x1fffffffffffffff8c1318ff606a089*h)
d=0xfffffffffffffffc6098c7fb035045
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffeffffedc7
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
