Curve "secq160k1", guessing x-value of the half of the generator, based on "secp160k1" and "secq256k1":
```
+---------+------------------------------------------------+
| p-value | 1 00000000 00000000 0001b8fa 16dfab9a ca16b6b3 |
| b-value |                                              7 |
| n-value |   ffffffff ffffffff ffffffff fffffffe ffffac73 |
+---------+------------------------------------------------+
| x-value |   243a3f87 28cfa747 95d06e74 cafa9a82 cef8e87f |
| y-value |   326598f2 59a19fbf f659367b bdf6c7f9 033452ff |
+---------+------------------------------------------------+
| (G/2).x |   48ce563f 89a0ed94 14f5aa28 ad0d96d6 795f9c63 |
| (G/2).y |   4e2f6173 1a699e79 e0ec1cb7 8d42b68e 5eeb634b |
+---------+------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x0100000000000000000001b8fa16dfab9aca16b6b3
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x243a3f8728cfa74795d06e74cafa9a82cef8e87f,0x326598f259a19fbff659367bbdf6c7f9033452ff)
h=1
E.set_order(0xfffffffffffffffffffffffffffffffeffffac73*h)
d=0x7fffffffffffffffffffffffffffffff7fffd63a
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
