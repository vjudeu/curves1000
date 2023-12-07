Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |   7fffff ffffffff fffffffe fffff28f |
| b-value |                                   3 |
| n-value |   7fffff ffffffff fb3131db adc8b629 |
+---------+-------------------------------------+
| x-value |   47ffff ffffffff ffffffff 6ffff86f |
| y-value |    9ffff ffffffff ffffffff ebfffef3 |
+---------+-------------------------------------+
| (G/2).x |                                   1 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffefffff28f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x47ffffffffffffffffffff6ffff86f,0x9ffffffffffffffffffffebfffef3)
h=1
E.set_order(0x7ffffffffffffffb3131dbadc8b629*h)
d=0x3ffffffffffffffd9898edd6e45b15
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffefffff28f
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
