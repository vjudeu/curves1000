Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |      fff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffa2fb |
| b-value |                                                                       7 |
| n-value |      fff ffffffff ffffffff ffffffff ffa32083 f30a0d46 490a3e6d d710097b |
+---------+-------------------------------------------------------------------------+
| x-value |      ccc cccccccc cccccccc cccccccc cccccccc cccccccc cccccccb ffffb594 |
| y-value |      55a 0cd4cd4c 19d6da8d ff9b75b5 76afc9a3 166e2016 da26ace9 5b5cbba6 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |      658 4bb51867 2373f0b7 16615156 9960f8cf be9d31a8 4ce388fb 728d121e |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffeffffa2fb
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xccccccccccccccccccccccccccccccccccccccccccccccccccbffffb594,0x55a0cd4cd4c19d6da8dff9b75b576afc9a3166e2016da26ace95b5cbba6)
h=1
E.set_order(0xfffffffffffffffffffffffffffffa32083f30a0d46490a3e6dd710097b*h)
d=0x7ffffffffffffffffffffffffffffd19041f98506a324851f36eb8804be
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffffffffffffffffffffeffffa2fb
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
