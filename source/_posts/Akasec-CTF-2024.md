---
title: 'Akasec CTF 2024 writeup'
thumbnail: '/images/Akasec-CTF-2024/akasec1.png'
date: 2024-06-11 12:46:20
tags:
  - 'CTF'
  - 'Cryptography'
---

My writeup on Akasec CTF on the challs solved by my team 'Bits & Pieces' and we placed rank 13th on this ctf event, it was a pretty good run this time!

## Crypto:

### Lost

In the given code:

```py
from random import getrandbits
from Crypto.Util.number import getPrime, bytes_to_long
from SECRET import FLAG

e = 2
p = getPrime(256)
q = getPrime(256)
n = p * q

m = bytes_to_long(FLAG)
cor_m = m - getrandbits(160)

if __name__ == "__main__":
    c = pow(m, e, n)
    print("n = {}\nc = {}\ncor_m = {}".format(n, c, cor_m))
```

We can identify that the `getrandbits(160)` is a small range and we can exploit this vulnerablility using the "Coppersmith's Attack" which helps us to find the potential value to decrypt the flag.

```py
m=724154397787031699242933363312913323086319394176220093419616667612889538090840511506381320998293481458429167685676367015744314015744
n=5113166966960118603250666870544315753374750136060769465485822149528706374700934720443689630473991177661169179462100732951725871457633686010946951736764639
e=2
c=329402637167950119278220170950190680807120980712143610290182242567212843996710001488280098771626903975534140478814872389359418514658167263670496584963653
P.<x> = PolynomialRing(Zmod(n), implementation='NTL')
pol = (m + x)**2 - c
roots = pol.small_roots(epsilon=1/50)
print("Potential solutions:")
for root in roots:
      a = root+m
      print(root+m)
```

Here the code attempts to find small roots of the polynomial using the small_roots method with an epsilon value of 1/50. The small_roots method is used to find roots of a polynomial where the roots are known to be small relative to the modulus n.

```py
roots = pol.small_roots(epsilon=1/50)
```

Then we iterate through the found roots, add each root to m, and prints the result. The addition of m to each root is done because the polynomial was defined as (m+x), so the roots found are offsets from m.

The encryption operation for RSA with exponent e can be written as: c = (m+k)<sup>2</sup> mod n

where k is some small value. The polynomial (m+x)<sup>2</sup> - c is constructed and the small roots method is used to find the possible values of k. By adding these roots back to m, the code attempts to recover the original plaintext m.

Flag: `AKASEC{c0pp3r5m17h_4774ck_1n_1ov3_w17h_5m4ll_3xp0n3nts}`

### Twin

In the given code:

```py
from Crypto.Util.number import getPrime, bytes_to_long, long_to_bytes
from SECRET import FLAG

e = 5
p = getPrime(256)
q = getPrime(256)
n = p * q

m1 = bytes_to_long(FLAG)
m2 = m1 >> 8

if __name__ == "__main__":
    c1, c2 = pow(m1, e, n), pow(m2, e, n)
    print("n = {}\nc1 = {}\nc2 = {}".format(n, c1, c2))
```

We need to exploit a specific relationship between the plaintext messages encrypted as c1 and c2 under the same RSA modulus and small exponent e=5. To find the missing value of the encrypted flag, let's say 'k', It iterates over potential values of k, calculates the GCD of the resulting polynomials, and checks for non-trivial solutions that can be used to derive the plaintext.

```py
e=5
n=6689395968128828819066313568755352659933786163958960509093076953387786003094796620023245908431378798689402141767913187865481890531897380982752646248371131
c1=3179086897466915481381271626207192941491642866779832228649829433228467288272857233211003674026630320370606056763863577418383068472502537763155844909495261
c2=6092690907728422411002652306266695413630015459295863614266882891010434275671526748292477694364341702119123311030726985363936486558916833174742155473021704
n1=n
n2=n

def my_gcd(a, b): 
    return a.monic() if b == 0 else my_gcd(b, a % b) 

R.<m2>=Zmod(n)[]
for k in range(256):
   f1=(m2*256+k)^e-c1
   f2=m2^e-c2
   if(my_gcd(f1, f2).coefficients()[0] != 1): # finds the potential
        res = (my_gcd(f1, f2).small_roots()[0] * 256) + k 
        print(long_to_bytes(Integer(res))) # prints the flag
```

If the leading coefficient of the GCD is not 1, it indicates a potential value which was at iteration 125 which converts to the ascii value `}`. Then uses the small_roots method to find the potential message.

```py
   if(my_gcd(f1, f2).coefficients()[0] != 1): # finds the potential
```

The message is converted into a readable format, and prints it.

Flag: `AKASEC{be_on_the_right_side_of_history_free_palestine}`

### My Calculus Lab

In the given code:

```py
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import hashlib
import sympy as sp
import random

FLAG = b'REDACTED'

key = random.getrandbits(7)

x = sp.symbols('x')

f = "REDACTED"
f_prime = "REDACTED"
f_second_prime = "REDACTED"

assert(2*f_second_prime - 6*f_prime + 3*f == 0)
assert(f.subs(x, 0) | f_prime.subs(x, 0) == 14)

def encrypt(message, key):
    global f
    global x
    point = f.subs(x, key).evalf(100)
    point_hash = hashlib.sha256(str(point).encode()).digest()[:16]
    cipher = AES.new(point_hash, AES.MODE_CBC)
    iv = cipher.iv
    ciphertext = cipher.encrypt(pad(message, AES.block_size))
    return iv.hex() + ciphertext.hex()

encrypted = encrypt(FLAG, key)

print(f"Key: {key}")
print(f"Encrypted: {encrypted}")
```

On observing the above code we can clearly see that:

- The function f satisfies a second-order linear differential equation: 2f'' - 6f- + 3f = 0
- Encryption key is derived by evaluating function f at a key value (7-bits)
- This key is then hashed using SHA-256 to generate the AES key.

To decrypt the message, we need to just reverse the process, that's to solve the differential equation with various initial conditions and regenerate the AES key.

```py
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad,unpad
import hashlib
import sympy as sp
import random
import math
from sympy import symbols, Function, Eq, dsolve, exp, Derivative

x = sp.symbols('x')
e=math.e
Key=60
Encrypted='805534c14e694348a67da0d75165623cf603c2a98405b34fe3ba8752ce24f5040c39873ec2150a61591b233490449b8b7bedaf83aa9d4b57d6469cd3f78fdf55'

iv_hex=Encrypted[:32]
ct=Encrypted[32:]

iv=bytes.fromhex(iv_hex)
c=bytes.fromhex(ct)

pos14=[[0, 14], [2, 12], [2, 14], [4, 10], [4, 14], [6, 8], [6, 10], [6, 12], [6, 14], [8, 6], [8, 14], [10, 4], [10, 6], [10, 12], [10, 14], [12, 2], [12, 6], [12, 10], [12, 14], [14, 0], [14, 2], [14, 4], [14, 6], [14, 8], [14, 10], [14, 12], [14, 14]]

x = symbols('x')
y = Function('y')(x)

# Define the differential equation 2y''-6y'+3y=0
differential_eq = Eq(2*y.diff(x, x) - 6*y.diff(x) + 3*y, 0)

for i in range(len(pos14)):
   # Define initial conditions y(0) = y0 and y'(0) = y1
   y0 = pos14[i][0]  # Ey(x)
   y1 = pos14[i][1]  # y'(x)
   initial_conditions = {y.subs(x, 0): y0, y.diff(x).subs(x, 0): y1}
   # Solve the differential equation with initial conditions
   solution = dsolve(differential_eq, y, ics=initial_conditions)
   f=solution.rhs
   point = f.subs(x, Key).evalf(100)
   point_hash = hashlib.sha256(str(point).encode()).digest()[:16]
   cipher = AES.new(point_hash, AES.MODE_CBC, iv)
   flag = cipher.decrypt(c)
   if b'AKASEC' in flag:
       print(flag)
```

We first define the differential Equation

```py
y = Function('y')(x)
differential_eq = Eq(2*y.diff(x, x) - 6*y.diff(x) + 3*y, 0)
```

Then iterate through all possible initial conditions, and every solution is evaluated and hashed at the key to derive the AES key.

```py
for i in range(len(pos14)):
   y0 = pos14[i][0]
   y1 = pos14[i][1]
   initial_conditions = {y.subs(x, 0): y0, y.diff(x).subs(x, 0): y1} 
   solution = dsolve(differential_eq, y, ics=initial_conditions) # Solve Diff eq 
   f=solution.rhs
   point = f.subs(x, Key).evalf(100) #evaluate
   point_hash = hashlib.sha256(str(point).encode()).digest()[:16] #hash
```

This regenerated AES derived key and IV is used to decrypt the cipher text.

```py
cipher = AES.new(point_hash, AES.MODE_CBC, iv)
flag = unpad(cipher.decrypt(c), AES.block_size)
```

Flag: `AKASEC{d1d_y0u_3nj0y_c41cu1u5_101?}`

Special thanks to `@ckc9759` for helping with the solutions and writeup!
