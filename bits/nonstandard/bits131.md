Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |        7 ffffffff ffffffff fffffffe ffffd8bb |
| b-value |                                            3 |
| n-value |        7 ffffffff fffffffc 13993e6f f09cb7a5 |
+---------+----------------------------------------------+
| x-value |        2 7fffffff ffffffff ffffffff affff3b9 |
| y-value |        6 1fffffff ffffffff ffffffff 3bffe1ef |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffeffffd8bb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x27fffffffffffffffffffffffaffff3b9,0x61fffffffffffffffffffffff3bffe1ef)
h=1
E.set_order(0x7fffffffffffffffc13993e6ff09cb7a5*h)
d=0x3fffffffffffffffe09cc9f37f84e5bd3
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffeffffd8bb
modulo_root=(p+1)/4
x=0
b_value=3
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
