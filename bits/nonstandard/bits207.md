Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |     7fff ffffffff ffffffff ffffffff ffffffff fffffffe ffffc9c7 |
| b-value |                                                              7 |
| n-value |     7fff ffffffff ffffffff ffffffe4 67d4f4f7 5e131980 1a844ea5 |
+---------+----------------------------------------------------------------+
| x-value |     43ff ffffffff ffffffff ffffffff ffffffff ffffffff 77ffe330 |
| y-value |     67e9 80a8d0ce 6dece568 3ab5ffb8 92a5d5e6 dcca9a05 c2dbafc9 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |     71e6 7f2a3159 e5a26114 f354a7d6 08e0b7c9 347100f6 cd4a8ddb |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffeffffc9c7
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x43ffffffffffffffffffffffffffffffffffffffffff77ffe330,0x67e980a8d0ce6dece5683ab5ffb892a5d5e6dcca9a05c2dbafc9)
h=1
E.set_order(0x7fffffffffffffffffffffffffe467d4f4f75e1319801a844ea5*h)
d=0x3ffffffffffffffffffffffffff233ea7a7baf098cc00d422753
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffeffffc9c7
modulo_root=(p+1)/4
x=0
b_value=7
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
