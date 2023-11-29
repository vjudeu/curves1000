Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |      fff ffffffff ffffffff ffffffff ffffffff fffffffe ffff510b |
| b-value |                                                              3 |
| n-value |     1000 00000000 00000000 00000021 137aeb15 b94b72aa df1b95d9 |
+---------+----------------------------------------------------------------+
| x-value |      4ff ffffffff ffffffff ffffffff ffffffff ffffffff afffc952 |
| y-value |       3f ffffffff ffffffff ffffffff ffffffff ffffffff fbfffd44 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffeffff510b
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x4ffffffffffffffffffffffffffffffffffffffffffafffc952,0x3ffffffffffffffffffffffffffffffffffffffffffbfffd44)
h=1
E.set_order(0x1000000000000000000000000021137aeb15b94b72aadf1b95d9*h)
d=0x80000000000000000000000001089bd758adca5b9556f8dcaed
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffeffff510b
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
