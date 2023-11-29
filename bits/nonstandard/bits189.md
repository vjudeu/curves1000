Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------+
| p-value | 1fffffff ffffffff ffffffff ffffffff fffffffe fffff08b |
| b-value |                                                     7 |
| n-value | 1fffffff ffffffff ffffffff 557e7959 c8043686 9caa746b |
+---------+-------------------------------------------------------+
| x-value |  fd1745d 1745d174 5d1745d1 745d1745 d1745d16 c745c9d1 |
| y-value |  c9ea16e 24904728 77ebf2ef 1c664e5e 463b6b8d 99f46d16 |
+---------+-------------------------------------------------------+
| (G/2).x |                                                     5 |
| (G/2).y |  73b46ea edc6c30c 7b8c7cd5 4a217885 fd660e6e c1dddd26 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffefffff08b
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xfd1745d1745d1745d1745d1745d1745d1745d16c745c9d1,0xc9ea16e2490472877ebf2ef1c664e5e463b6b8d99f46d16)
h=1
E.set_order(0x1fffffffffffffffffffffff557e7959c80436869caa746b*h)
d=0xfffffffffffffffffffffffaabf3cace4021b434e553a36
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffffffffffffffefffff08b
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
