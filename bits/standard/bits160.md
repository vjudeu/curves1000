Parameters from https://neuromancer.sk/std/secg/secp160k1
```
+---------+------------------------------------------------+
| p-value |   ffffffff ffffffff ffffffff fffffffe ffffac73 |
| b-value |                                              7 |
| n-value | 1 00000000 00000000 0001b8fa 16dfab9a ca16b6b3 |
+---------+------------------------------------------------+
| x-value |   3b4c382c e37aa192 a4019e76 3036f4f5 dd4d7ebb |
| y-value |   938cf935 318fdced 6bc28286 531733c3 f03c4fee |
+---------+------------------------------------------------+
| (G/2).x |   48ce563f 89a0ed94 14f5aa28 ad0d96d6 795f9c62 |
| (G/2).y |   bcda9d4a cd74573f 55010aef 6c7b4b1a c4daa8ca |
+---------+------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffeffffac73
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x3b4c382ce37aa192a4019e763036f4f5dd4d7ebb,0x938cf935318fdced6bc28286531733c3f03c4fee)
h=1
E.set_order(0x0100000000000000000001b8fa16dfab9aca16b6b3*h)
d=0x80000000000000000000dc7d0b6fd5cd650b5b5a
P=d*G
print(hex(P[0]),hex(P[1]))
```
