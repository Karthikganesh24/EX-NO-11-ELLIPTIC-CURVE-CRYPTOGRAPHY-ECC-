# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

def mod_inverse(a, m):
    m0, x0, x1 = m, 0, 1
    if m == 1:
        return 0
    while a > 1:
        q = a // m
        a, m = m, a % m
        x0, x1 = x1 - q * x0, x0
    return x1 + m0 if x1 < 0 else x1

def point_addition(P, Q, a, p):
    if P.x == Q.x and P.y == Q.y:
        num = (3 * P.x**2 + a)
        den = (2 * P.y)
    else:
        num = (Q.y - P.y)
        den = (Q.x - P.x)

    lam = (num * mod_inverse(den, p)) % p
    rx = (lam**2 - P.x - Q.x) % p
    ry = (lam * (P.x - rx) - P.y) % p
    return Point(rx, ry)

def scalar_multiplication(P, k, a, p):
    result = P
    k -= 1
    while k > 0:
        result = point_addition(result, P, a, p)
        k -= 1
    return result

def main():
    print("Elliptic Curve Diffie-Hellman (ECDH) Key Exchange\n")
    p = int(input("Enter the prime number (p): "))
    a = int(input("Enter curve parameter a: "))
    b = int(input("Enter curve parameter b: "))
    gx = int(input("Enter x-coordinate of base point G: "))
    gy = int(input("Enter y-coordinate of base point G: "))
    G = Point(gx, gy)
    privateA = int(input("Enter Alice's private key: "))
    privateB = int(input("Enter Bob's private key: "))
    publicA = scalar_multiplication(G, privateA, a, p)
    publicB = scalar_multiplication(G, privateB, a, p)
    print(f"Alice's public key: ({publicA.x}, {publicA.y})")
    print(f"Bob's public key: ({publicB.x}, {publicB.y})")
    sharedA = scalar_multiplication(publicB, privateA, a, p)
    sharedB = scalar_multiplication(publicA, privateB, a, p)

    print(f"Shared secret computed by Alice: ({sharedA.x}, {sharedA.y})")
    print(f"Shared secret computed by Bob: ({sharedB.x}, {sharedB.y})")

    if sharedA.x == sharedB.x and sharedA.y == sharedB.y:
        print("Key exchange successful. Both shared secrets match!")
    else:
        print("Key exchange failed. Shared secrets do not match.")

if __name__ == "__main__":
    main()
```


## Output:
![image](https://github.com/user-attachments/assets/e37e221d-d865-4157-adc6-7a06a1262bb0)


## Result:
The program is executed successfully

