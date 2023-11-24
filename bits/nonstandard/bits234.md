Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |      3ff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffff8c15 |
| b-value |                                                                       5 |
| n-value |      3ff ffffffff ffffffff ffffffff ffc4b5e5 3d714de7 3b82bbe4 a4125039 |
+---------+-------------------------------------------------------------------------+
| x-value |      1bc d081bcd0 81bcd081 bcd081bc d081bcd0 81bcd081 bcd081bc 614d6a08 |
| y-value |      30c 0f812df0 6ad40eed 0a00cfb3 781d6e95 42e5400b 3ba4f285 28efb487 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       6 |
| (G/2).y |      208 7c48f5f6 d2bf8ef5 e7ab2972 3bfacca6 518aaf11 a314b065 9329919f |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffeffff8c15
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1bcd081bcd081bcd081bcd081bcd081bcd081bcd081bcd081bc614d6a08,0x30c0f812df06ad40eed0a00cfb3781d6e9542e5400b3ba4f28528efb487)
h=1
E.set_order(0x3ffffffffffffffffffffffffffffc4b5e53d714de73b82bbe4a4125039*h)
d=0x1ffffffffffffffffffffffffffffe25af29eb8a6f39dc15df25209281d
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3fffffffffffffffffffffffffffffffffffffffffffffffffeffff8c15
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=0
b_value=5
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    first_y=y_square.powermod(first_modulo_root,p)
    is_on_curve=(first_y.powermod(2,p)==y_square)
    y=first_y
    if not is_on_curve:
        second_y=(first_y*second_multiplier)%p
        is_on_curve=(second_y.powermod(2,p)==y_square)
        y=second_y
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
