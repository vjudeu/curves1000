Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |  3ffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffc8f |
| b-value |                                                              3 |
| n-value |  3ffffff ffffffff ffffffff ffffffec 36d4285d cb3fb401 fb678a11 |
+---------+----------------------------------------------------------------+
| x-value |  23fffff ffffffff ffffffff ffffffff ffffffff ffffffff 6ffffe0f |
| y-value |   4fffff ffffffff ffffffff ffffffff ffffffff ffffffff ebffffbb |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |                                                              2 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffefffffc8f
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0x23fffffffffffffffffffffffffffffffffffffffffffff6ffffe0f,0x4fffffffffffffffffffffffffffffffffffffffffffffebffffbb)
h=1
E.set_order(0x3ffffffffffffffffffffffffffffec36d4285dcb3fb401fb678a11*h)
d=0x1fffffffffffffffffffffffffffff61b6a142ee59fda00fdb3c509
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffefffffc8f
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
