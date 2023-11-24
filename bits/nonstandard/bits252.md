Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |  fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff79f7 |
| b-value |                                                                       5 |
| n-value |  fffffff ffffffff ffffffff ffffffff 80d4ce69 52c22581 bf54a584 dee77b43 |
+---------+-------------------------------------------------------------------------+
| x-value |  c4ec4ec 4ec4ec4e c4ec4ec4 ec4ec4ec 4ec4ec4e c4ec4ec4 ec4ec4eb 89d8366e |
| y-value |  62a6a0c 9954034f 2efa69bf 05b56233 ee3597b7 03f044ec d02e0ed5 1a4e5d6d |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |  bc87b66 c2fe56ee 5666bea3 cada30d2 9b012ba9 145b57e8 5ac6a5e9 796b2763 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffffffeffff79f7
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xc4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4ec4eb89d8366e,0x62a6a0c9954034f2efa69bf05b56233ee3597b703f044ecd02e0ed51a4e5d6d)
h=1
E.set_order(0xfffffffffffffffffffffffffffffff80d4ce6952c22581bf54a584dee77b43*h)
d=0x7ffffffffffffffffffffffffffffffc06a6734a96112c0dfaa52c26f73bda2
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffffffeffff79f7
modulo_root=(p+1)/4
x=0
b_value=5
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
