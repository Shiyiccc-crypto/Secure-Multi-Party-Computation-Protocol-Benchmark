from Crypto.Util import number
import random

# Step 1: Generate n random integers as private keys for n participants, store in array x
def generate_random_numbers(n, q):
    x = [random.randint(n, q) for _ in range(n)]
    return x

# Step 2: Compute modular exponentiation with base g and exponents from array x, simulating public keys, store in array X
def calculate_mod_pow(x, g, p):
    X = [pow(g, i, p) for i in x]
    return X

# Step 3: Add X0 and Xn+1 by prepending and appending 1 to array X, creating new array X2
def add_elements_to_ends(X):
    X2 = [1] + X + [1]
    return X2

# Step 4: Compute intermediate values A and B needed for array Y calculation using array X2
def calculate_mod_multiply(X2, p):
    n = len(X2)
    A = [1] * (n-2)
    B1 = [1] * (n-2)
    X3 = X2[::-1]  # Reverse X2
    for i in range(n - 3):
        A[i + 1] = (A[i] * X2[i+1]) % p
    for i in range(n - 3):
        B1[i + 1] = (B1[i] * X3[i+1]) % p
    B = B1[::-1]  # Reverse B1 to get B
    return A, B

# Step 5: Perform modular division of A by B (element-wise) using modular inverses, result in array Y
def calculate_modulo_division(A, B, p):
    # Initialize result array Y
    Y = []

    # Perform element-wise modular division and add to Y
    for i in range(len(A)):
        # Compute A[i] / B[i] mod p
        result = (A[i] * mod_inverse(B[i], p)) % p
        Y.append(result)

    return Y

def extended_gcd(a, b):
    if a == 0:
        return b, 0, 1
    else:
        gcd, x, y = extended_gcd(b % a, a)
        return gcd, y - (b // a) * x, x

def mod_inverse(a, m):
    gcd, x, _ = extended_gcd(a, m)
    if gcd != 1:
        raise ValueError(f"The inverse of {a} modulo {m} does not exist.")
    return x % m

# Step 6: Compute modular exponentiation with bases from Y and exponents from x, result in array B0
def calculate_mod_pow_with_Y(Y, x, p):
    B0 = [pow(Y[i], x[i], p) for i in range(len(Y))]
    return B0

# Step 7: Generate random binary array v of length n (participants' private summation values)
def generate_binary_array(n):
    v = [random.choice([0, 1]) for _ in range(n)]
    return v

# Step 8: Compute modular exponentiation with base g and exponents from array y, result in array G
def calculate_mod_pow_with_y(g, y, p):
    G = [pow(g, i, p) for i in y]
    return G

# Step 9: Compute element-wise modular multiplication of B0 and G, result in array B1
def calculate_multiply_with_G(B0, G, p):
    B1 = [(B0[i] * G[i]) % p for i in range(len(B0))]
    return B1

# Step 10: Compute product of all elements in B1 modulo p, final result B
def calculate_final_result(B1, p):
    B = 1
    for element in B1:
        B = (B * element) % p
    return B

# Step 11: Decrypt using Baby-Step Giant-Step algorithm to solve discrete logarithm
def baby_step_giant_step(g, B2, p):
    m = n  # Typically m = ceil(sqrt(n)), but using n as in original code

    # Precompute the values in the baby-step phase
    baby_steps = {pow(g, j, p): j for j in range(m)}

    # Precompute the value used in the giant-step phase
    gamma = pow(g, m * (p - 2), p)  # gamma = g^(-m) mod p

    # Search for the match in the giant-step phase
    for i in range(m):
        y = (B2 * pow(gamma, i, p)) % p
        if y in baby_steps:
            return i * m + baby_steps[y]

    return None  # No solution found

# Set protocol parameters: number of participants n, large prime p, large prime factor q of p-1, generator g
n = 10
q = 63762351364972653564641699529205510489263266834182771617563631363277932854227
g = 8041367327046189302693984665026706374844608289874374425728797669509435881459140662650215832833471328470334064628508692231999401840332046192569287351991689963279656892562484773278584208040987631569628520464069532361274047374444344996651832979378318849943741662110395995778429270819222431610927356005913836932462099770076239554042855287138026806960470277326229482818003962004453764400995790974042663675692120758726145869061236443893509136147942414445551848162391468541444355707785697825741856849161233887307017428371823608125699892904960841221593344499088996021883972185241854777608212592397013510086894908468466292313
p = 17125458317614137930196041979257577826408832324037508573393292981642667139747621778802438775238728592968344613589379932348475613503476932163166973813218698343816463289144185362912602522540494983090531497232965829536524507269848825658311420299335922295709743267508322525966773950394919257576842038771632742044142471053509850123605883815857162666917775193496157372656195558305727009891276006514000409365877218171388319923896309377791762590614311849642961380224851940460421710449368927252974870395873936387909672274883295377481008150475878590270591798350563488168080923804611822387520198054002990623911454389104774092183

# Parameter generation phase

# Generate private keys x for n participants
x = generate_random_numbers(n, q)
# Generate three arrays of n random numbers r, r1, r2 for ZKP
r = generate_random_numbers(n, p-1)
r1 = generate_random_numbers(n, p-1)
r2 = generate_random_numbers(n, p-1)

# Time the first phase (parameter selection)
import time
start_time1 = time.time()

# Compute public keys X for n participants
X = calculate_mod_pow(x, g, p)

# Generate first ZKP: 1 commitment, 1 challenge, and 1 response for each participant
Commitment = calculate_mod_pow(r, g, p)
Challenge = generate_random_numbers(n, p-1)
Responses = [(c - (ch * xi) % (p-1)) % (p-1) for c, ch, xi in zip(r, Challenge, x)]

end_time1 = time.time()
duration1 = (end_time1 - start_time1) / n
print(f"Average computation time per participant in parameter selection phase: {duration1} seconds")

# Generate masking arrays for DDH in the second phase
alpha1 = generate_random_numbers(n, p-1)
Alpha1 = calculate_mod_pow(alpha1, g, p)
beta1 = generate_random_numbers(n, p-1)
Beta1 = calculate_mod_pow(beta1, g, p)
cei1 = calculate_multiply_with_G(alpha1, beta1, p-1)
Cei1 = calculate_mod_pow(cei1, g, p)

# Time the second phase (data submission)
start_time2 = time.time()

# Compute Y values for each participant
X2 = add_elements_to_ends(X)
A, B = calculate_mod_multiply(X2, p)
Y = calculate_modulo_division(A, B, p)

# Generate participants' private summation values v
v = generate_binary_array(n)

# Compute participants' encrypted data b
B0 = calculate_mod_pow_with_Y(Y, x, p)
G = calculate_mod_pow_with_y(g, v, p)
B1 = calculate_multiply_with_G(B0, G, p)

# Generate second ZKP: 8 commitments, 2 challenges, and 4 responses for each participant
Commitment1 = calculate_mod_pow(r1, g, p)
Commitment2 = calculate_mod_pow(r2, g, p)
Commitment3 = [pow(Y_i, r1_i, p) for Y_i, r1_i in zip(Y, r1)]
Commitment4 = [pow(Beta1_i, r2_i, p) for Beta1_i, r2_i in zip(Beta1, r2)]
Challenge1 = generate_random_numbers(n, p-1)
Challenge2 = generate_random_numbers(n, p-1)
Responses1 = [(r1_i - (ch1_i * x_i) % (p-1)) % (p-1) for r1_i, ch1_i, x_i in zip(r1, Challenge1, x)]
Responses2 = [(r2_i - (ch1_i * alpha1_i) % (p-1)) % (p-1) for r2_i, ch1_i, alpha1_i in zip(r2, Challenge1, alpha1)]
Responses3 = generate_random_numbers(n, p-1)
Responses4 = generate_random_numbers(n, p-1)

g_Responses3 = calculate_mod_pow(Responses3, g, p)
X_ch2 = [pow(X_i, ch2_i, p) for X_i, ch2_i in zip(X, Challenge2)]
Commitment5 = calculate_multiply_with_G(g_Responses3, X_ch2, p)

g_Responses4 = calculate_mod_pow(Responses4, g, p)
Alpha1_ch2 = [pow(Alpha1_i, ch2_i, p) for Alpha1_i, ch2_i in zip(Alpha1, Challenge2)]
Commitment6 = calculate_multiply_with_G(g_Responses4, Alpha1_ch2, p)

Y_Responses3 = [pow(Y_i, Responses3_i, p) for Y_i, Responses3_i in zip(Y, Responses3)]
B0_ch2 = [pow(B0_i, ch2_i, p) for B0_i, ch2_i in zip(B0, Challenge2)]
Commitment7 = calculate_multiply_with_G(Y_Responses3, B0_ch2, p)

Beta1_Responses4 = [pow(Beta1_i, Responses4_i, p) for Beta1_i, Responses4_i in zip(Beta1, Responses4)]
Cei1_ch2 = [pow(Cei1_i, ch2_i, p) for Cei1_i, ch2_i in zip(Cei1, Challenge2)]
Commitment8 = calculate_multiply_with_G(Beta1_Responses4, Cei1_ch2, p)

end_time2 = time.time()
duration2 = (end_time2 - start_time2) / n
print(f"Average computation time per participant in data submission phase: {duration2} seconds")

# Time the third phase (summation/aggregation)
start_time3 = time.time()

# Compute the product of all encrypted data B
B2 = calculate_final_result(B1, p)

# Compute the exponent S (the summation result) using Baby-Step Giant-Step
S = baby_step_giant_step(g, B2, p)
print("Summation result: S =", S)
end_time3 = time.time()
duration3 = (end_time3 - start_time3) 
print(f"Average computation time per participant in summation phase: {duration3} seconds")

# Time the fourth phase (verification)
start_time4 = time.time()

# Verify the first ZKP for all participants
Left = calculate_mod_pow(Responses, g, p)
X_D = [pow(X_i, Challenge_i, p) for X_i, Challenge_i in zip(X, Challenge)]
Right = calculate_modulo_division(Commitment, X_D, p)
if Left == Right:
    print("All participants' first ZKPs passed verification")
else:
    for i in range(len(Left)):
        if Left[i] != Right[i]:
            print(f"Participant {i + 1}'s first ZKP failed verification")

# Verify the second ZKP for all participants
Left1 = calculate_mod_pow(Responses1, g, p)
X_ch1 = [pow(X_i, Challenge1_i, p) for X_i, Challenge1_i in zip(X, Challenge1)]
Right1 = calculate_modulo_division(Commitment1, X_ch1, p)

Left2 = calculate_mod_pow(Responses2, g, p)
Alpha1_ch1 = [pow(Alpha1_i, Challenge1_i, p) for Alpha1_i, Challenge1_i in zip(Alpha1, Challenge1)]
Right2 = calculate_modulo_division(Commitment2, Alpha1_ch1, p)

Left3 = [pow(Y_i, Responses1_i, p) for Y_i, Responses1_i in zip(Y, Responses1)]
B0_ch1 = [pow(B0_i, Challenge1_i, p) for B0_i, Challenge1_i in zip(B0, Challenge1)]
Right3 = calculate_modulo_division(Commitment3, B0_ch1, p)
            
Left4 = [pow(Beta1_i, Responses2_i, p) for Beta1_i, Responses2_i in zip(Beta1, Responses2)]
Cei1_ch1 = [pow(Cei1_i, Challenge1_i, p) for Cei1_i, Challenge1_i in zip(Cei1, Challenge1)]
Right4 = calculate_modulo_division(Commitment4, Cei1_ch1, p)
            
Left5 = calculate_mod_pow(Responses3, g, p)
X_ch2 = [pow(X_i, Challenge2_i, p) for X_i, Challenge2_i in zip(X, Challenge2)]
Right5 = calculate_modulo_division(Commitment5, X_ch2, p)
            
Left6 = calculate_mod_pow(Responses4, g, p)
Alpha1_ch2 = [pow(Alpha1_i, Challenge2_i, p) for Alpha1_i, Challenge2_i in zip(Alpha1, Challenge2)]
Right6 = calculate_modulo_division(Commitment6, Alpha1_ch2, p)
            
Left7 = [pow(Y_i, Responses3_i, p) for Y_i, Responses3_i in zip(Y, Responses3)]
B0_ch2 = [pow(B0_i, Challenge2_i, p) for B0_i, Challenge2_i in zip(B0, Challenge2)]
Right7 = calculate_modulo_division(Commitment7, B0_ch2, p)
            
Left8 = [pow(Beta1_i, Responses4_i, p) for Beta1_i, Responses4_i in zip(Beta1, Responses4)]
Cei1_ch2 = [pow(Cei1_i, ch2_i, p) for Cei1_i, ch2_i in zip(Cei1, Challenge2)]
Right8 = calculate_modulo_division(Commitment8, Cei1_ch2, p)

Left_arrays = [globals()[f"Left{i}"] for i in range(1, 9)]
Right_arrays = [globals()[f"Right{i}"] for i in range(1, 9)]

all_passed = True
for i in range(8):
    for j in range(n):
        if Left_arrays[i][j] != Right_arrays[i][j]:
            print(f"Participant {j+1}'s equation {i+1} failed verification.")
            all_passed = False

if all_passed:
    print("All participants' second ZKPs passed verification.")

end_time4 = time.time()
duration4 = (end_time4 - start_time4) 

print(f"Third-party verifier computation time: {duration4} seconds")
