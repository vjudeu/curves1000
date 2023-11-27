Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |  fffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffe873 |
| b-value |                                                              3 |
| n-value |  fffffff ffffffff ffffffff ffff800a 0448100c 5920413a 2d25388b |
+---------+----------------------------------------------------------------+
| x-value |  cffffff ffffffff ffffffff ffffffff ffffffff ffffffff 2fffecdc |
| y-value |  23fffff ffffffff ffffffff ffffffff ffffffff ffffffff dbfffcb0 |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffeffffe873
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xcffffffffffffffffffffffffffffffffffffffffffffff2fffecdc,0x23fffffffffffffffffffffffffffffffffffffffffffffdbfffcb0)
h=1
E.set_order(0xfffffffffffffffffffffffffff800a0448100c5920413a2d25388b*h)
d=0x7ffffffffffffffffffffffffffc005022408062c90209d16929c46
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffeffffe873
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
