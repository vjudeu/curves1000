Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |       7f ffffffff ffffffff ffffffff ffffffff fffffffe ffffbbc3 |
| b-value |                                                              5 |
| n-value |       80 00000000 00000000 00000005 723800ca b66490b4 a64031ed |
+---------+----------------------------------------------------------------+
| x-value |       6f ffffffff ffffffff ffffffff ffffffff ffffffff 1fffc449 |
| y-value |       68 26bfa778 0bdb6899 43edd456 9ecfdbfb cea54717 2f1fc30d |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              1 |
| (G/2).y |       2e 1ae9bbba 0c0d5b59 68ef0aa6 ec2ef469 49c219e8 5fa2dfee |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffeffffbbc3
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x6fffffffffffffffffffffffffffffffffffffffff1fffc449,0x6826bfa7780bdb689943edd4569ecfdbfbcea547172f1fc30d)
h=1
E.set_order(0x80000000000000000000000005723800cab66490b4a64031ed*h)
d=0x40000000000000000000000002b91c00655b32485a532018f7
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffeffffbbc3
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
