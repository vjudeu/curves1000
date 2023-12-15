Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------+
| p-value |     7fff ffffffff fffffffe ffffff43 |
| b-value |                                   5 |
| n-value |     7fff ffffffff ff0b2923 0ca8142b |
+---------+-------------------------------------+
| x-value |     67ff ffffffff ffffffff 2fffff69 |
| y-value |     71ff ffffffff ffffffff 1bffff53 |
+---------+-------------------------------------+
| (G/2).x |     7fff ffffffff fffffffe ffffff42 |
| (G/2).y |                                   2 |
+---------+-------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffeffffff43
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x67ffffffffffffffffff2fffff69,0x71ffffffffffffffffff1bffff53)
h=1
E.set_order(0x7fffffffffffff0b29230ca8142b*h)
d=0x3fffffffffffff85949186540a16
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffeffffff43
modulo_root=(p+1)/4
x=p
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
        x-=1
```
