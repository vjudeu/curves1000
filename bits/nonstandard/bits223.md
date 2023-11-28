Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value | 7fffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffffbb |
| b-value |                                                              3 |
| n-value | 7fffffff ffffffff ffffffff ffffa1b7 b1cef3c7 9fdbffd2 aa8ee347 |
+---------+----------------------------------------------------------------+
| x-value | 27ffffff ffffffff ffffffff ffffffff ffffffff ffffffff afffffe9 |
| y-value | 61ffffff ffffffff ffffffff ffffffff ffffffff ffffffff 3bffffcb |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffeffffffbb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x27ffffffffffffffffffffffffffffffffffffffffffffffafffffe9,0x61ffffffffffffffffffffffffffffffffffffffffffffff3bffffcb)
h=1
E.set_order(0x7fffffffffffffffffffffffffffa1b7b1cef3c79fdbffd2aa8ee347*h)
d=0x3fffffffffffffffffffffffffffd0dbd8e779e3cfedffe9554771a4
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffeffffffbb
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
