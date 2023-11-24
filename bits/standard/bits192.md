Parameters from https://neuromancer.sk/std/secg/secp192k1
```
+---------+-------------------------------------------------------+
| p-value | ffffffff ffffffff ffffffff ffffffff fffffffe ffffee37 |
| b-value |                                                     3 |
| n-value | ffffffff ffffffff fffffffe 26f2fc17 0f69466a 74defd8d |
+---------+-------------------------------------------------------+
| x-value | db4ff10e c057e9ae 26b07d02 80b7f434 1da5d1b1 eae06c7d |
| y-value | 9b2f2f6d 9c5628a7 844163d0 15be8634 4082aa88 d95e2f9d |
+---------+-------------------------------------------------------+
| (G/2).x |  554123b 78ce563f 89a0ed94 14f5aa28 ad0d96d6 795f9c66 |
| (G/2).y | 90755fc9 3ea4b130 27fc6c8f 337d92c6 d00090fa d0e8b2c6 |
+---------+-------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffeffffee37
K=GF(p)
a=K(0)
b=K(3)
E=EllipticCurve(K,(a,b))
G=E(0xdb4ff10ec057e9ae26b07d0280b7f4341da5d1b1eae06c7d,0x9b2f2f6d9c5628a7844163d015be86344082aa88d95e2f9d)
h=1
E.set_order(0xfffffffffffffffffffffffe26f2fc170f69466a74defd8d*h)
d=0x7fffffffffffffffffffffff13797e0b87b4a3353a6f7ec7
P=d*G
print(hex(P[0]),hex(P[1]))
```
