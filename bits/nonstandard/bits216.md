Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |   ffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff9d73 |
| b-value |                                                              3 |
| n-value |   ffffff ffffffff ffffffff ffffe828 680a4b9c d4a15b7b 5b7feb75 |
+---------+----------------------------------------------------------------+
| x-value |   cfffff ffffffff ffffffff ffffffff ffffffff ffffffff 2fffafec |
| y-value |   23ffff ffffffff ffffffff ffffffff ffffffff ffffffff dbfff224 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffeffff9d73
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xcfffffffffffffffffffffffffffffffffffffffffffff2fffafec,0x23ffffffffffffffffffffffffffffffffffffffffffffdbfff224)
h=1
E.set_order(0xffffffffffffffffffffffffffe828680a4b9cd4a15b7b5b7feb75*h)
d=0x7ffffffffffffffffffffffffff414340525ce6a50adbdadbff5bb
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffeffff9d73
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
