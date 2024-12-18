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
   - Use any program that can run Python scripts, I personnally use PyCharm.
   
2. **Choose to Encrypt:**

   ![alt text](https://github.com/KheyDheNs/DES_Encryption/blob/main/images/1.png?raw=true)
   - Enter `1` at the menu to encrypt.     

3. **Provide Plaintext:**
   - Once done, the code will ask you to provide a plaintext.
   
   ![alt text](https://github.com/KheyDheNs/DES_Encryption/blob/main/images/2.png?raw=true)
   - Input an 8-character string (here, I took `ABCD1234`).

4. **Provide a Key:**
   - The next step is to get a key. You'll have a choice to generate a random key or to enter one by yourself.
     
   ![alt text](https://github.com/KheyDheNs/DES_Encryption/blob/main/images/3.png?raw=true)
   - If you choose to generate a random key, you'll get a random hexadecimal key in response.
   
   ![alt text](https://github.com/KheyDheNs/DES_Encryption/blob/main/images/4.png?raw=true)
   - Otherwise, you have to input your own 8-character key (here, I chose `abcd1234`).

   ![alt text](https://github.com/KheyDheNs/DES_Encryption/blob/main/images/5.png?raw=true)

6. **View the Output:**
   - Finally, the program will display the newly encrypted cyphertext (in hexadecimal). My result here is `aed3cb7f0f484056`.

   ![alt text](https://github.com/KheyDheNs/DES_Encryption/blob/main/images/6.png?raw=true)

7. **Redo the process or exit:**
   - The script will relaunch itself automatically after generating the results.
   - You can either choose `1` to try again with different values, or `2` to exit it

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
