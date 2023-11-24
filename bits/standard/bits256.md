Parameters from https://neuromancer.sk/std/secg/secp256k1
```
+---------+-------------------------------------------------------------------------+
| p-value | ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffc2f |
| b-value |                                                                       7 |
| n-value | ffffffff ffffffff ffffffff fffffffe baaedce6 af48a03b bfd25e8c d0364141 |
+---------+-------------------------------------------------------------------------+
| x-value | 79be667e f9dcbbac 55a06295 ce870b07 029bfcdb 2dce28d9 59f2815b 16f81798 |
| y-value | 483ada77 26a3c465 5da4fbfc 0e1108a8 fd17b448 a6855419 9c47d08f fb10d4b8 |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                         3b 78ce563f 89a0ed94 14f5aa28 ad0d96d6 795f9c63 |
| (G/2).y | c0c68640 8d517dfd 67c23676 51380d00 d126e422 9631fd03 f8ff35ee f1a61e3c |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffefffffc2f
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x79be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798,0x483ada7726a3c4655da4fbfc0e1108a8fd17b448a68554199c47d08ffb10d4b8)
h=1
E.set_order(0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141*h)
d=0x7fffffffffffffffffffffffffffffff5d576e7357a4501ddfe92f46681b20a1
P=d*G
print(hex(P[0]),hex(P[1]))
```
