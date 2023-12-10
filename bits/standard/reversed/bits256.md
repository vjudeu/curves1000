Parameters from https://docs.rs/ark-secq256k1/latest/ark_secq256k1/
```
+---------+-------------------------------------------------------------------------+
| p-value | ffffffff ffffffff ffffffff fffffffe baaedce6 af48a03b bfd25e8c d0364141 |
| b-value |                                                                       7 |
| n-value | ffffffff ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe fffffc2f |
+---------+-------------------------------------------------------------------------+
| x-value | 76c39f55 85cb160e b6b06c87 a2ce32e2 3134e45a 097781a6 a24288e3 7702eda6 |
| y-value | 3ffc646c 7b2918b5 dc2d265a 8e82a7f7 d18983d2 6e8dc055 a4120dda d952677f |
+---------+-------------------------------------------------------------------------+
| (G/2).x |                         3b 78ce563f 89a0ed94 14f5aa28 ad0d96d6 795f9c63 |
| (G/2).y | d13ee134 27211551 5be539ea 7779c821 fb1b3bc6 87c4d7d8 94ae3b70 a2ca394e |
+---------+-------------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x76c39f5585cb160eb6b06c87a2ce32e23134e45a097781a6a24288e37702eda6,0x3ffc646c7b2918b5dc2d265a8e82a7f7d18983d26e8dc055a4120ddad952677f)
h=1
E.set_order(0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffefffffc2f*h)
d=0x7fffffffffffffffffffffffffffffffffffffffffffffffffffffff7ffffe18
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
