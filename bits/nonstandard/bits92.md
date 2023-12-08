Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |  fffffff fffffffe ffffc50f |
| b-value |                          3 |
| n-value | 10000000 00007ca5 7fe03059 |
+---------+----------------------------+
| x-value |  8ffffff ffffffff 6fffded7 |
| y-value |  13fffff ffffffff ebfffb65 |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |                          2 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffeffffc50f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x8ffffffffffffff6fffded7,0x13fffffffffffffebfffb65)
h=1
E.set_order(0x1000000000007ca57fe03059*h)
d=0x800000000003e52bff0182d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffeffffc50f
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
