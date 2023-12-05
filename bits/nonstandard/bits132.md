Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |        f ffffffff ffffffff fffffffe ffffe3ab |
| b-value |                                            5 |
| n-value |        f ffffffff fffffff8 6e7744e9 6b706ced |
+---------+----------------------------------------------+
| x-value |        d ffffffff ffffffff ffffffff 1fffe734 |
| y-value |        e 10a841c5 41c90df7 a112fd16 bc853354 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |        1 43e7fad9 6f2b7a46 e59a3701 0a1f1ffd |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffeffffe3ab
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xdffffffffffffffffffffffff1fffe734,0xe10a841c541c90df7a112fd16bc853354)
h=1
E.set_order(0xffffffffffffffff86e7744e96b706ced*h)
d=0x7fffffffffffffffc373ba274b5b83677
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffeffffe3ab
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
