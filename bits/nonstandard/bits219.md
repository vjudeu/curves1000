Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |  7ffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffc073 |
| b-value |                                                              3 |
| n-value |  7ffffff ffffffff ffffffff ffffaa58 6e4ece82 096488a0 6ed321b5 |
+---------+----------------------------------------------------------------+
| x-value |  67fffff ffffffff ffffffff ffffffff ffffffff ffffffff 2fffcc5c |
| y-value |  11fffff ffffffff ffffffff ffffffff ffffffff ffffffff dbfff710 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffeffffc073
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x67fffffffffffffffffffffffffffffffffffffffffffff2fffcc5c,0x11fffffffffffffffffffffffffffffffffffffffffffffdbfff710)
h=1
E.set_order(0x7ffffffffffffffffffffffffffaa586e4ece82096488a06ed321b5*h)
d=0x3ffffffffffffffffffffffffffd52c3727674104b24450376990db
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffffffffffffffffffffeffffc073
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
