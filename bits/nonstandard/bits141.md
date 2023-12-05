Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------+
| p-value |     1fff ffffffff ffffffff fffffffe fffffa3f |
| b-value |                                            7 |
| n-value |     2000 00000000 000000ab c57d6958 3288eabd |
+---------+----------------------------------------------+
| x-value |      8ff ffffffff ffffffff ffffffff b7fffe60 |
| y-value |      3fc e944291f e7c4eff7 58322880 3ee213e7 |
+---------+----------------------------------------------+
| (G/2).x |                                            1 |
| (G/2).y |      8f2 ec14121a b604351a d5c89a01 01421fce |
+---------+----------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0x1ffffffffffffffffffffffffffefffffa3f
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0x8ffffffffffffffffffffffffffb7fffe60,0x3fce944291fe7c4eff7583228803ee213e7)
h=1
E.set_order(0x200000000000000000abc57d69583288eabd*h)
d=0x10000000000000000055e2beb4ac1944755f
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0x1ffffffffffffffffffffffffffefffffa3f
modulo_root=(p+1)/4
x=0
b_value=7
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
