Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value | 1fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffdc03 |
| b-value |                                                                       7 |
| n-value | 1fffffff ffffffff ffffffff ffffffff 4da897ac ab769b43 2821e4c1 1c5403cd |
+---------+-------------------------------------------------------------------------+
| x-value |  6666666 66666666 66666666 66666666 66666666 66666666 66666666 33332bff |
| y-value |   450e07 347aca7c ee90ed77 96f52d31 8fe3d566 b091ba6e c84450db 1b6bb0ab |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       2 |
| (G/2).y |  66e65b3 48b9fabe d860a755 9ce766b6 759170a3 914b3358 de6cb7c3 3e51d0cb |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffffffffeffffdc03
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x666666666666666666666666666666666666666666666666666666633332bff,0x450e07347aca7cee90ed7796f52d318fe3d566b091ba6ec84450db1b6bb0ab)
h=1
E.set_order(0x1fffffffffffffffffffffffffffffff4da897acab769b432821e4c11c5403cd*h)
d=0xfffffffffffffffffffffffffffffffa6d44bd655bb4da19410f2608e2a01e7
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffffffffffffffffffeffffdc03
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
