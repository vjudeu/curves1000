Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------+
| p-value |  7ffffff fffffffe ffffe287 |
| b-value |                          3 |
| n-value |  8000000 000042d2 e56a3ed3 |
+---------+----------------------------+
| x-value |   7fffff ffffffff effffe27 |
| y-value |  39fffff ffffffff 8bfff2a5 |
+---------+----------------------------+
| (G/2).x |                          1 |
| (G/2).y |                          2 |
+---------+----------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffeffffe287
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x7fffffffffffffeffffe27,0x39fffffffffffff8bfff2a5)
h=1
E.set_order(0x8000000000042d2e56a3ed3*h)
d=0x40000000000216972b51f6a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffeffffe287
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
