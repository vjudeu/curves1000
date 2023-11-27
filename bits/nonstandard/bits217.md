Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |  1ffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff83cb |
| b-value |                                                              3 |
| n-value |  1ffffff ffffffff ffffffff ffffdf8d 3365b1d6 942138d8 f063e83d |
+---------+----------------------------------------------------------------+
| x-value |   9fffff ffffffff ffffffff ffffffff ffffffff ffffffff afffd92e |
| y-value |    7ffff ffffffff ffffffff ffffffff ffffffff ffffffff fbfffe0f |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffeffff83cb
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x9fffffffffffffffffffffffffffffffffffffffffffffafffd92e,0x7fffffffffffffffffffffffffffffffffffffffffffffbfffe0f)
h=1
E.set_order(0x1ffffffffffffffffffffffffffdf8d3365b1d6942138d8f063e83d*h)
d=0xffffffffffffffffffffffffffefc699b2d8eb4a109c6c7831f41f
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffeffff83cb
modulo_root=(p+1)/4
x=0
b_value=3
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
