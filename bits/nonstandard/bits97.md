Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |        1 ffffffff fffffffe ffffeb7b |
| b-value |                                   5 |
| n-value |        1 ffffffff fffe68d5 733adc65 |
+---------+-------------------------------------+
| x-value |          9fffffff ffffffff affff999 |
| y-value |          87ffffff ffffffff bbfffa88 |
+---------+-------------------------------------+
| (G/2).x |        1 ffffffff fffffffe ffffeb7a |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffeffffeb7b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x9fffffffffffffffaffff999,0x87ffffffffffffffbbfffa88)
h=1
E.set_order(0x1fffffffffffe68d5733adc65*h)
d=0xffffffffffff346ab99d6e33
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffeffffeb7b
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
