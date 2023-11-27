Non-standard elliptic curve, based on secp256k1, with non-standard dummy generator:
```
+---------+----------------------------------------------------------------+
| p-value |       ff ffffffff ffffffff ffffffff ffffffff fffffffe ffffe123 |
| b-value |                                                              7 |
| n-value |       ff ffffffff ffffffff fffffff3 081da398 bc51211f b6d55725 |
+---------+----------------------------------------------------------------+
| x-value |       fe 8ba2e8ba 2e8ba2e8 ba2e8ba2 e8ba2e8b a2e8ba2d 8d172722 |
| y-value |       5f eaf7ace8 4242577b b7a265ea c90bfd40 a961f27c 066302ad |
+---------+----------------------------------------------------------------+
| (G/2).x |                                                              5 |
| (G/2).y |       1b bc1d0677 c69c84f1 a23c3000 e179a793 e2f9b8cb 9b29c9a9 |
+---------+----------------------------------------------------------------+
```
Sage code for testing half of the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffeffffe123
K=GF(p)
a=K(0)
b=K(7)
E=EllipticCurve(K,(a,b))
G=E(0xfe8ba2e8ba2e8ba2e8ba2e8ba2e8ba2e8ba2e8ba2d8d172722,0x5feaf7ace84242577bb7a265eac90bfd40a961f27c066302ad)
h=1
E.set_order(0xfffffffffffffffffffffffff3081da398bc51211fb6d55725*h)
d=0x7ffffffffffffffffffffffff9840ed1cc5e28908fdb6aab93
print(hex(d))
P=d*G
print(hex(P[0]),hex(P[1]))
```
Sage code for finding the generator:
```
p=0xfffffffffffffffffffffffffffffffffffffffffeffffe123
modulo_root=(p+1)/4
x=0
b_value=7
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
