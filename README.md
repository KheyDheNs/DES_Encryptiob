# Howto: Implement and understand my Single-Block DES

This guide provides a step-by-step explanation of a **single-block DES implementation**, simplified yet adhering to the original algorithm. It covers the main components and flow of the encryption process.

---

## **What is DES?**
DES (Data Encryption Standard) is a symmetric key block cipher. It operates on a 64-bit block of plaintext using a 56-bit key (with 8 parity bits). The encryption involves transformations like permutations, substitutions, and the Feistel network structure, repeated over 16 rounds.

---

## **Why Single-Block DES is Acceptable**

This implementation stays true to the core **DES algorithm**, focusing on encrypting a single block of data. While real-world applications often involve multi-block encryption (for example with ECB or CBC modes), this single-block encryption shows the fundamental mechanics of DES effectively.

---

## **How the Code Works**

### 1. **Input**
- **Plaintext:** An 8-character string (64 bits).
- **Key:** Another 8-character string (64 bits), of which only 56 bits are used.

---

### 2. **Key Generation**
- **Key Permutation (PC1):**
  - The initial key is permuted using the `PC1` table to select 56 bits.
- **Splitting into Halves:**
  - The permuted key is split into two halves of 28 bits each.
- **Shifts and Round Key Generation (PC2):**
  - The halves are rotated left by a specified number of positions (`SHIFT_SCHEDULE`) and recombined.
  - Another permutation (`PC2`) selects 48 bits from the combined key to create **16 round keys**.

---

### 3. **Initial Permutation (IP)**
- The plaintext is permuted using the `IP` table to reorder its bits.

---

### 4. **Splitting the Text**
- The permuted plaintext is split into:
  - **Left Half (L):** First 32 bits.
  - **Right Half (R):** Last 32 bits.

---

### 5. **Feistel Network (16 Rounds)**
Each round involves:
1. **Expansion:**
   - The 32-bit right half is expanded to 48 bits using the `E` table.
2. **Key Mixing:**
   - The expanded bits are XORed with the current round key.
3. **Substitution (S-Boxes):**
   - The 48 bits are divided into eight 6-bit chunks. Each chunk passes through a corresponding **S-Box**, which maps it to a 4-bit output based on the chunk's row and column.
4. **Permutation (P):**
   - The 32 bits from the S-Box output are permuted.
5. **Swapping:**
   - The result is XORed with the left half, and the halves are swapped for the next round.

---

### 6. **Final Permutation (FP)**
- After 16 rounds, the left and right halves are combined in reverse order (R + L) and permuted using the `FP` table.

---

### 7. **Output**
- The final result is a 64-bit encrypted block, which is converted to hexadecimal for readability.

---

## **How to Use the Code**

1. **Run the Program:**
  

2. **Choose to Encrypt:**
  - Enter `1` at the menu.

3. **Provide Plaintext:**
  - Input an 8-character string (here, I took `ABCD1234`).

4. **Provide a Key:**
  - Optionally generate a random key or input your own 8-character key (here, I chose `abcd1234`).

5. **View the Output:**
  - The program will display the generated/encrypted key and the encrypted ciphertext in hexadecimal format.

---

## **Example Walkthrough**

### Input:
  - **Plaintext:** `ABCD1234`
  - **Key:** `abcd1234`

### Process:
1. **Plaintext to Bits:**
  - `ABCD1234` is converted to binary (64 bits).
2. **Initial Permutation:**
  - Bits are reordered using the **IP table**.
3. **Key Generation:**
  - The key `abcd1234` is permuted and shifted to generate 16 round keys.
4. **Feistel Rounds:**
  - Each round transforms the plaintext block using the round keys.
5. **Final Permutation:**
  - After 16 rounds, the final block is permuted to produce the ciphertext.

### Output:
  - **Encrypted Result:** `aed3cb7f0f484056`

---

## **Simplified Explanation for Each Step**

1. **Key Generation:**
  - DES derives 16 round keys from your input key through bit permutations and shifts.

2. **Initial Permutation (IP):**
  - The plaintext bits are shuffled to prepare for encryption.

3. **Rounds (Feistel Structure):**
  - DES uses 16 rounds of processing:
    - The **right half** of the block is transformed.
    - The **left half** is swapped with the transformed result.

4. **Final Permutation (FP):**
  - The output bits are reordered to produce the final ciphertext.
