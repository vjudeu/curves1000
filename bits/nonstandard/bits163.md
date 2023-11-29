Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |        7 ffffffff ffffffff ffffffff fffffffe ffff9a57 |
| b-value |                                                     5 |
| n-value |        7 ffffffff ffffffff fffc7e61 ed88c184 48798f7b |
+---------+-------------------------------------------------------+
| x-value |          9d89d89d 89d89d89 d89d89d8 9d89d89d 76275aa3 |
| y-value |        5 28df3cb4 25937ec7 c756b21a 6af79418 f4ae6ef0 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     2 |
| (G/2).y |        3 daeea3b2 a90a64d7 8ca11a73 7a2f9fab ef1fa56f |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffeffff9a57
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x9d89d89d89d89d89d89d89d89d89d89d76275aa3,0x528df3cb425937ec7c756b21a6af79418f4ae6ef0)
h=1
E.set_order(0x7fffffffffffffffffffc7e61ed88c18448798f7b*h)
d=0x3fffffffffffffffffffe3f30f6c460c2243cc7be
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffeffff9a57
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
