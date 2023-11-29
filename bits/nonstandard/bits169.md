Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |      1ff ffffffff ffffffff ffffffff fffffffe ffffe923 |
| b-value |                                                     5 |
| n-value |      1ff ffffffff ffffffff ffd5f032 20b2b891 54839423 |
+---------+-------------------------------------------------------+
| x-value |      1bf ffffffff ffffffff ffffffff ffffffff 1fffebfd |
| y-value |      147 5788c6b2 905ec473 27858dd2 e147018a 706c0c2c | 
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |       76 472c499b 7349676b 2fefd7c9 138e8725 5bdcc40f |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffeffffe923
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1bfffffffffffffffffffffffffffffffff1fffebfd,0x1475788c6b2905ec47327858dd2e147018a706c0c2c)
h=1
E.set_order(0x1ffffffffffffffffffffd5f03220b2b89154839423*h)
d=0xffffffffffffffffffffeaf81910595c48aa41ca12
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffeffffe923
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
