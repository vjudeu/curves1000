Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value | 1fffffff fffffffe ffffe71f |
| b-value |                          5 |
| n-value | 20000000 00002795 7b951207 |
+---------+----------------------------+
| x-value | 1d89d89d 89d89d88 ec4eadf4 |
| y-value |  fdac97e 433066b3 ea89de2c |
+---------+----------------------------+
| (G/2).x |                          2 |
| (G/2).y |  738db0a 57750ece bba0817f |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffeffffe71f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1d89d89d89d89d88ec4eadf4,0xfdac97e433066b3ea89de2c)
h=1
E.set_order(0x20000000000027957b951207*h)
d=0x10000000000013cabdca8904
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffeffffe71f
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
