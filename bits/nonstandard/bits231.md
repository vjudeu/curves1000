Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |       7f ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffebe7 |
| b-value |                                                                       5 |
| n-value |       80 00000000 00000000 00000000 000a4493 0f71ae49 01a983ef 0b154215 |
+---------+-------------------------------------------------------------------------+
| x-value |       40 ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 7dfff5cb |
| y-value |       18 e91f0011 39b8dbfc d3ea776c 46371e18 5ec36993 3ffb56cb 89697dfb |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       3 |
| (G/2).y |       2b 9dae5738 c85a5756 37c2647d f5ec2926 990c10ff 9a7935a1 445b01d6 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffeffffebe7
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x40ffffffffffffffffffffffffffffffffffffffffffffffff7dfff5cb,0x18e91f001139b8dbfcd3ea776c46371e185ec369933ffb56cb89697dfb)
h=1
E.set_order(0x80000000000000000000000000000a44930f71ae4901a983ef0b154215*h)
d=0x400000000000000000000000000005224987b8d72480d4c1f7858aa10b
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x7ffffffffffffffffffffffffffffffffffffffffffffffffeffffebe7
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
