Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |    7ffff ffffffff ffffffff fffffffe ffffec0b |
| b-value |                                            5 |
| n-value |    7ffff ffffffff fffffb93 5611bfd2 63824941 |
+---------+----------------------------------------------+
| x-value |    27fff ffffffff ffffffff ffffffff affff9c6 |
| y-value |    41fff ffffffff ffffffff ffffffff 7bfff5b1 |
+---------+----------------------------------------------+
| (G/2).x |    7ffff ffffffff ffffffff fffffffe ffffec0a |
| (G/2).y |                                            2 |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7fffffffffffffffffffffffffffeffffec0b
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x27fffffffffffffffffffffffffffaffff9c6,0x41fffffffffffffffffffffffffff7bfff5b1)
h=1
E.set_order(0x7fffffffffffffffffb935611bfd263824941*h)
d=0x3fffffffffffffffffdc9ab08dfe931c124a1
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7fffffffffffffffffffffffffffeffffec0b
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
