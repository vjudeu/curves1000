Parameters from https://neuromancer.sk/std/secg/secp224k1
```
+---------+------------------------------------------------------------------+
| p-value |   ffffffff ffffffff ffffffff ffffffff ffffffff fffffffe ffffe56d |
| b-value |                                                                5 |
| n-value | 1 00000000 00000000 00000000 0001dce8 d2ec6184 caf0a971 769fb1f7 |
+---------+------------------------------------------------------------------+
| x-value |   a1455b33 4df099df 30fc28a1 69a467e9 e47075a9 0f7e650e b6b7a45c |
| y-value |   7e089fed 7fba3442 82cafbd6 f7e319f7 c0b0bd59 e2ca4bdb 556d61a5 |
+---------+------------------------------------------------------------------+
| (G/2).x |                  3b 78ce563f 89a0ed94 14f5aa28 ad0d96d6 795f9c63 |
| (G/2).y |   f7d94781 b93cd624 22e8a244 72ffda23 418d7c0e 25ca89ed 909f9ce4 |
+---------+------------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffffffffeffffe56d
K=GF(p)
a=K(0)
b=K(5)
E=EllipticCurve(K,(a,b))
G=E(0xa1455b334df099df30fc28a169a467e9e47075a90f7e650eb6b7a45c,0x7e089fed7fba344282cafbd6f7e319f7c0b0bd59e2ca4bdb556d61a5)
h=1
E.set_order(0x10000000000000000000000000001dce8d2ec6184caf0a971769fb1f7*h)
d=0x8000000000000000000000000000ee74697630c2657854b8bb4fd8fc
P=d*G
print(hex(P[0]),hex(P[1]))
```
