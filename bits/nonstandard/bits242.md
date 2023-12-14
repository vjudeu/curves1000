Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |    3ffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff740f |
| b-value |                                                                       5 |
| n-value |    40000 00000000 00000000 00000000 00287729 fe3ece24 6e3eeb44 b91f411f |
+---------+-------------------------------------------------------------------------+
| x-value |    23fff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6fffb14b |
| y-value |    24fff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 6bffaf14 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |    3ffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff740e |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffeffff740f
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x23fffffffffffffffffffffffffffffffffffffffffffffffffff6fffb14b,0x24fffffffffffffffffffffffffffffffffffffffffffffffffff6bffaf14)
h=1
E.set_order(0x4000000000000000000000000000000287729fe3ece246e3eeb44b91f411f*h)
d=0x2000000000000000000000000000000143b94ff1f6712371f75a25c8fa090
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffffeffff740f
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
