Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |     7fff ffffffff ffffffff ffffffff ffffffff fffffffe ffffc9c7 |
| b-value |                                                              7 |
| n-value |     7fff ffffffff ffffffff ffffffe4 67d4f4f7 5e131980 1a844ea5 |
+---------+----------------------------------------------------------------+
| x-value |     43ff ffffffff ffffffff ffffffff ffffffff ffffffff 77ffe330 |
| y-value |     1816 7f572f31 92131a97 c54a0047 6d5a2a19 233565f9 3d2419fe |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |      e19 80d5cea6 1a5d9eeb 0cab5829 f71f4836 cb8eff08 32b53bec |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffeffffc9c7
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x43ffffffffffffffffffffffffffffffffffffffffff77ffe330,0x18167f572f3192131a97c54a00476d5a2a19233565f93d2419fe)
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
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
