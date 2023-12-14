Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value | 3fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff76d |
| b-value |                                                                       5 |
| n-value | 40000000 00000000 00000000 00000000 f5dfe982 e92ec378 aab02ef2 e75f6507 |
+---------+-------------------------------------------------------------------------+
| x-value |  bffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff cffffe67 |
| y-value | 36ffffff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 23fff89d |
+---------+-------------------------------------------------------------------------+
| (G/2).x | 3fffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffff76c |
| (G/2).y |                                                                       2 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffffffefffff76d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xbffffffffffffffffffffffffffffffffffffffffffffffffffffffcffffe67,0x36ffffffffffffffffffffffffffffffffffffffffffffffffffffff23fff89d)
h=1
E.set_order(0x40000000000000000000000000000000f5dfe982e92ec378aab02ef2e75f6507*h)
d=0x200000000000000000000000000000007aeff4c1749761bc5558177973afb284
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x3ffffffffffffffffffffffffffffffffffffffffffffffffffffffefffff76d
first_modulo_root=(p+3)/8
second_modulo_root=(p-1)/4
two=2
second_multiplier=two.powermod(second_modulo_root,p)
x=p
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
    if is_on_curve:
        y_negative=(p-y)
        if y_negative<y:
            y=y_negative
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x-=1
```
