Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |       7f fffffffe fffff4ab |
| b-value |                          7 |
| n-value |       80 00000010 81b7b7c7 |
+---------+----------------------------+
| x-value |        4 b4b4b4b4 ab4b4ae0 |
| y-value |       31 86937aae ad5b5d09 |
+---------+----------------------------+
| (G/2).x |                          3 |
| (G/2).y |       2e 55095eb2 9b7c3ac8 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffefffff4ab
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x4b4b4b4b4ab4b4ae0,0x3186937aaead5b5d09)
h=1
E.set_order(0x800000001081b7b7c7*h)
d=0x400000000840dbdbe4
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffefffff4ab
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
