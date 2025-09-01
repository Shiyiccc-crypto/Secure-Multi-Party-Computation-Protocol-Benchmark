# Efficient and Secure Multiparty Summation without Semi-Honest Third-Party

This repository contains the official Python implementation of the privacy-preserving secure summation protocol presented in the paper **"Efficient and Secure Multiparty Summation without Semi-Honest Third-Party"** by Liu et al. The protocol allows multiple parties to compute the sum of their private binary values (`0` or `1`) without revealing any individual value, leveraging cryptographic primitives like Zero-Knowledge Proofs (ZKP) and homomorphic encryption.

## 📖 Abstract

> In modern distributed computing systems, ensuring the security and privacy of data across numerous distributed devices is paramount. We propose an efficient and verifiable multiparty summation protocol using public-key cryptography. Each participant can securely share encrypted data with attached validity proofs, enabling anyone to independently verify and compute the final result. This decentralized protocol eliminates the need for a semi-honest third party, enhancing resilience against active attacks from malicious participants and observers. Additionally, it removes the requirement for secret channels, making it ideal for public networks. This design significantly reduces overhead while ensuring robust security. Experimental results demonstrate the efficiency and scalability of our protocol, highlighting its potential for practical applications in privacy-preserving computations across medical, military, and commercial domains.

## 🚀 Features

- **Secure Multi-Party Computation (MPC)**: Enables the computation of a sum over private data.
- **Zero-Knowledge Proofs (ZKP)**: Allows participants to prove the correctness of their computations without revealing secrets.
- **Cryptographic Operations**: Implements modular exponentiation, modular inverses, and the Baby-Step Giant-Step algorithm for discrete logarithms.
- **Simulation Ready**: Provides a complete simulation of the protocol's workflow, including all parties and a verifier.

---

## ⚙️ Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Shiyiccc-crypto/Secure-Multi-Party-Computation-Protocol-Benchmark.git
    cd Shiyiccc-crypto
    ```

2.  **(Recommended) Create a virtual environment:**
    ```bash
    # Using conda
    conda create -n myenv python=3.12
    conda activate myenv
    ```

3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *The `requirements.txt` file only requires `pycryptodome`. You should create it:*
    ```bash
    echo "pycryptodome==3.19.0" > requirements.txt
    ```

---

## 🧪 Usage & Reproducibility

The main simulation script is contained in a single Python file. The code is structured to follow the exact steps of the cryptographic protocol.

### Running the Complete Protocol Simulation

To run the entire protocol simulation with the default parameters (n=10 parties), simply execute:

```bash
python main.py
```

### Output Explanation

Running the script will output the following key results and performance metrics:
- **Average time per participant in the Parameter Generation phase.**
- **Average time per participant in the Data Submission phase.**
- **The final summation result `S`.**
- **The total time for the Summation phase.**
- **The total time for the Verification phase.**
- **Confirmation messages for all ZKP verifications.**

### Reproducing Paper Experiments

To reproduce specific figures or tables from the paper (e.g., scalability plots):

1. **Modify Parameters:** The core parameters are defined at the top of the script:
    ```python
    n = 10  # Number of participants
    q = 63762...  # A large prime factor of p-1
    g = 80413...  # Generator
    p = 17125...  # Large prime modulus
    ```
    To test scalability, change `n` to different values (e.g., 10, 50, 100) and run the script each time.


2.  **Record Data:** The script prints timing results to the console. Redirect the output to a file for analysis:
    ```bash
    python main.py > results_n10.txt
    ```
---

## 📁 Repository Structure

```
.
├── main.py              # The main script implementing the entire protocol
├── requirements.txt     # Python dependencies
└── README.md           # This file
```

The core protocol is implemented in `main.py` with clearly labeled steps:
- **Step 1-2:** Key Generation (Private & Public Keys)
- **Step 3-5:** Preparation of Masking Values (Y)
- **Step 6-10:** Data Encryption and Submission
- **Step 11:** Aggregation and Decryption (Baby-Step Giant-Step)
- **ZKP Generation & Verification:** Interleaved throughout the process.

---

## 🛠️ Dependencies

- **Python 3.12**
- **PyCryptodome:** `pip install pycryptodome`
  - *Used for secure random number generation and cryptographic utilities. While the core math is implemented from scratch, this library provides a robust foundation.*

---

## ❓ Contact

For any questions regarding this code or the underlying protocol, please open an issue on this GitHub repository or contact [Liu] at [liuzhuomathbox@163.com].

---

## ⚠️ Important Notes

- **This is a simulation:** This code is designed for research and simulation purposes. Deployment in a real-world, adversarial environment would require additional security considerations, network communication layers, and thorough audits.
- **Parameter Selection:** The security of the protocol relies heavily on the appropriate selection of the large primes `p` and `q` and the generator `g`. The provided values are examples.
- **Performance:** The Baby-Step Giant-Step algorithm has a time complexity of O(√n), making it suitable for the sums simulated in the paper but may become slow for extremely large modulus sizes or result ranges.
