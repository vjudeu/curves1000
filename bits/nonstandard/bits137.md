Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |      1ff ffffffff ffffffff fffffffe fffff0df |
| b-value |                                            5 |
| n-value |      200 00000000 00000021 ac389041 dc338a65 |
+---------+----------------------------------------------+
| x-value |       e3 ffffffff ffffffff ffffffff 8dfff943 |
| y-value |       bc 4bdd1ad2 6f1950cf 8b8998ff 6e00e25d |
+---------+----------------------------------------------+
| (G/2).x |                                            3 |
| (G/2).y |       ef dd6791d2 a45d15a3 ac0e84b6 a238060c |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffefffff0df
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xe3ffffffffffffffffffffffff8dfff943,0xbc4bdd1ad26f1950cf8b8998ff6e00e25d)
h=1
E.set_order(0x2000000000000000021ac389041dc338a65*h)
d=0x1000000000000000010d61c4820ee19c533
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffefffff0df
modulo_root=(p+1)/4
x=0
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
        x+=1
```
