Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value |        f ffffffff ffffffff ffffffff fffffffe ffffe2a3 |
| b-value |                                                     5 |
| n-value |        f ffffffff ffffffff ffff6141 cb09ce9e 27fc368b |
+---------+-------------------------------------------------------+
| x-value |        d ffffffff ffffffff ffffffff ffffffff 1fffe64d |
| y-value |        5 10adf757 30bb1f3d aaac19db 04426050 bec75a03 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     1 |
| (G/2).y |        5 a0eea4d3 cf540235 55512925 c5105c2b 1ad331de |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xffffffffffffffffffffffffffffffffeffffe2a3
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xdffffffffffffffffffffffffffffffff1fffe64d,0x510adf75730bb1f3daaac19db04426050bec75a03)
h=1
E.set_order(0xfffffffffffffffffffff6141cb09ce9e27fc368b*h)
d=0x7ffffffffffffffffffffb0a0e584e74f13fe1b46
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xffffffffffffffffffffffffffffffffeffffe2a3
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
