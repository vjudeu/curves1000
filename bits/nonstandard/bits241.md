Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+-------------------------------------------------------------------------+
| p-value |    1ffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffae13 |
| b-value |                                                                       5 |
| n-value |    20000 00000000 00000000 00000000 00ebbf12 d72c5804 5dbccfbe 6de77513 |
+---------+-------------------------------------------------------------------------+
| x-value |    1bfff ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff 1fffb84f |
| y-value |    163f6 17d9841d dc461a91 d54b6cae 56f438f7 5b3c7086 48640fd0 242e50da |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                                                                       1 |
| (G/2).y |    1c5ee 46416863 50a86ce3 64f609a7 bd3988d3 53ad444a d0514659 c549c960 |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffffffeffffae13
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0x1bfffffffffffffffffffffffffffffffffffffffffffffffffff1fffb84f,0x163f617d9841ddc461a91d54b6cae56f438f75b3c708648640fd0242e50da)
h=1
E.set_order(0x2000000000000000000000000000000ebbf12d72c58045dbccfbe6de77513*h)
d=0x100000000000000000000000000000075df896b962c022ede67df36f3ba8a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1fffffffffffffffffffffffffffffffffffffffffffffffffffeffffae13
modulo_root=(p+1)/4
x=0
b_value=5
is_on_curve=False
while not is_on_curve:
    x_cube=(x*x*x)%p
    y_square=(x_cube+b_value)%p
    y=y_square.powermod(modulo_root,p)
    is_on_curve=(y.powermod(2,p)==y_square)
    print(is_on_curve,hex(x),hex(y))
    if not is_on_curve:
        x+=1
```
