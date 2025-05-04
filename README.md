# Computational Theory Assignment

## Table of Contents
- [Project Overview](#project-overview)
- [Task 1: Binary Representations](#task-1-binary-representations)
- [Task 2: Hash Functions](#task-2-hash-functions)
- [Task 3: SHA256 Padding](#task-3-sha256-padding)
- [Task 4: Prime Numbers](#task-4-prime-numbers)
- [Task 5: Roots](#task-5-roots)
- [Task 6: Proof of Work](#task-6-proof-of-work)
- [Task 7: Turing Machines](#task-7-turing-machines)
- [Task 8: Computational Complexity](#task-8-computational-complexity)
- [Usage Instructions](#usage-instructions)
- [Installation](#installation)
- [Dependencies](#dependencies)
- [Project Structure](#project-structure)
- [References by Task](#research-and-references)


## Project Overview
This project covers some core topics from computational theory, working through eight different tasks, each focusing on different areas such as binary operations, hash functions, padding, prime generation, Turing machines, and sorting algorithms. The goal of this project was to build a solid understanding of how these concepts are applied in computing by researching each topic and demonstrating how they work with testing to prove that the logic implemented works. It is a hands-on breakdown of some core ideas behind modern computing. 

## Task 1: Binary Representations

### Overview
Binary representations are fundamental in computing and cryptography. In this task, we implement several functions to work with 32-bit unsigned integers using bitwise operations. These operations are crucial for tasks like data manipulation in hash functions (e.g., SHA-256) and encryption algorithms.

### Functions Implemented
- **Left Rotation (`rotl`)** - Rotates the bits of a number to the left by a given amount, wrapping around the overflow.
- **Right Rotation (`rotr`)** - Rotates bits to the right, same idea as rotl, but in the other direction.
- **Bitwise Choice (`ch`)** - A conditional selector that picks bits from two numbers based on a third (used in SHA-256).
- **Bitwise Majority (`maj`)** - Returns the majority bit of three inputs — whichever value, either 0 or 1, occurs most often at each position.

### Usage Instructions
The functions can be tested with any 32-bit integer. All of them use Python's built-in bitwise operators

---

## Task 2: Hash Functions

### Overview
In this task, we convert a simple hash function from C into Python. The original function was taken from The C Programming Language by Kernighan and Ritchie. The goal of this task is to understand how a low-level C algorithm can be implemented using Python syntax and features, while also preserving the core logic of the algorithm.

### Functions Implemented
- **simple_hash(s)** - Implements the same logic similar to the C function. For each character in the string, the ASCII value is obtained using the `ord(char)`, which is used in the following formula:
  `hashval = (ord(char) + 31 * hashval) % 10)`.
- This ensures the hash is always in the range between 0 and 100.

### Usage Instructions
To use this function, simply pass any string to `simple_hash`. This function will return an integer hash value between 0 and 100. To validate that the function works correctly, I have included test cases with edge cases such as an empty string.

---

## Task 3: SHA256 Padding

### Overview
This task implements SHA-256 padding, which is an important step in preparing input data for the SHA-256 algorithm because SHA-256 requires the input messages to be padded so that their length is a multiple of 512 bits.The padding process follows the specification outlined in the NIST FIPS PUB 180-4 standard [1].

The padding process works as follows:
- Attaching a single '1' bit (represented as 0x80 in hex).
- Adding enough zero bits so that the total length matches 448 modulo 512.
- Attaching the original message length as a 64-bit big-endian integer.

### Functions Implemented
- `sha256_padding(file_path)`: Reads a file, calculates the SHA-256 padding, and outputs the resulting padding in hexadecimal format.

### Usage Instructions
To test the function:
1. Create a file with any content (e.g. `sample.txt`).
2. Call the `sha256_padding("sample.txt")` to check the padding output.

### Example output
```text
80 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 18
```
---

## Task 4: Prime Numbers

### Overview
This task involves generating the first 100 prime numbers using two different approaches, one being the iterative method and the other the Sieve of Eratosthenes. Prime numbers play a central role in number theory and cryptography, and this task demonstrates the logic behind prime checking and also highlights the differences in performance between naive and optimised methods.

### Methods Implemented
- **Iterative Prime Checking**:
- This method checks each number one by one, and it tests if it is divisible by any of the other primes identified.
- It is a straightforward method, but it can be slower for larger inputs.

-**Sieve of Eratosthenes**:
- Sieve of Eratosthenes is an algorithm that can eliminate non-prime numbers by marking multiples of each prime in a boolean array.
- It can be very effective for generating a large range of primes.

### Methods Implemented
- **`is_prime(n, primes)**: This is a helper function that tests if a number is prime based on a list of found primes.
- **`generate_primes_iterative(limit)`**:  Generates a limited number of primes using iterative prime checking.
- **`generate_primes_sieve(limit)`**: Generates a limited number of primes using the Sieve of Eratosthenes.

### Usage Instructions
- Both `generate_primes_iterative(100)` or `generate_primes_sieve(100)` to get the first 100 prime numbers.
- The outputs from both of these methods are printed and compared to confirm their correctness.
- An Assertion is also included to ensure both methods return the same results.
---

## Task 5: Roots

### Overview
This task calculates the first 32 bits of the fractional part of the square roots of the first 100 primes. This task provides insights into how mathematical constants can be represented in binary form. This sort of process is used in cryptgraphic applications where these constants are used in algorithm initialisation.

### Steps Performed
- Generate the first 100 primes using the iterative method from task 4.
- So for each prime:
  - Compute the square root using math.sqrt().
  - Get the fractional part by subtracting the floor value.
  - Multiply the fractional part by 2^32 and convert it into an integer.
  - Finally, convert the integer to a 32-bit binary string.

### Functions Implemented
- **`fractional_bits_of_sqrt(n, bits=32)`**: Takes the number `n` and it returns the first `bits` of the fractional part of its square root in binary form.
  
### Usage Instructions
- Use `generate_primes_iterative(100)` to get the first 100 primes.
- To get the binary representation, apply fractional_bits_of_sqrt(prime) for each of the prime numbers.
- The output then shows each prime with its corresponding 32-bit fractional binary string.
---

## Task 6: Proof of Work

### Overview
Task 6 simulates a basic proof-of-work implementation that computes SHA-356 hashes for English words, and it determines which words contain the highest number of leading zero bits in their binary digest. This mimics the hash difficulty conditions, which are often seen in cryptocurrencies like Bitcoin.

### Steps Performed
- Load a list of English words from words_alpha.txt.
- Using Python's `hashlib` library to compute the SHA-256 digest for each word.
- Convert each digest to a 256-bit binary string.
- Count how many zero bits appear at the start of each binary string.
- Then track and also store the words with the highest number of leading zeros.
- Finally, check if the words with the highest count of zero bits exist in an English dictionary using WordNet.

### Functions Implemented
- **`load_words(file_path)`**: Loads English words from a text file.
- **`SHA256_to_binary(word)`**: Calculates the SHA-256 digest of a word and converts it to binary.
- **`count_leading_zeros(binary_string)`**: Counts the number of leading zero bits in a binary string.

### Usage Instructions
- Firstly, call `load_words()` to import the word list.
- Then use a loop to process each word using the hashing and counting functions.
- Display the words with the maximum number of leading zeros.
- Finally, verify the results using `wordnet.synsets()` from the NLTK corpus.
---

## Task 7: Turing Machines

### Overview
This task demonstrates a basic Turing Machine, which is a foundational model of computation introduced by Alan Turing. A Turing machine works on an infinite tape and reads and writes symbols based on a set of transition rules. This simulation shows how simple rules can drive the machine through different types of states before it stops.

### Steps Performed
- Firstly, define the machine using a 7-tuple: `(Q, T, b, A, δ, s, F)`.
- Write a transition function that instructs the machine on what to do.
- Simulate how the tape progresses as the machine processes the inputs.
- Check how the machine stops in either an accepting or rejecting final state.

### Functions Implemented
- **`delta(state, symbol)`**: Defines the transition logic for the machine.
- **`simulate(table, initial_input, verbose=True)`**: With a provided state table and input, the machine runs the simulation.

### Usage Instructions
- For demonstration, a sample state table is provided.
- Call `simulate(state_table, '1010') or any other binary string of choice.
- The function then prints each step until the machine reaches a final state.
---

## Task 8: Computational Complexity

### Overview
This task demonstrates the computational complexity using the Bubble Sort Algorithm. The goal of this task is to understand how the number of comparisons changes depending on the input order, which alligns with the definition of time complexity as described in theoretical science.

Bubble Sort is well-suited for this demonstration because of the following:
- It is easy to understand and implement.
- The comparison behaviour is predictable.
- It demonstrates best-case, worst-case, and average-case scenarios.

### Steps Performed
- Implemented a version of Bubble Sort that counts the number of comparisons.
- Apply this function to every permutation from the list `[1, 2, 3, 4, 5]`, which totals to 120 permutations.
- Finally, print the number of comparisons for each of the permutations.

### Functions Implemented
- **`bubble_sort_with_count(arr)`**: This function sorts a list using Bubble Sort and it returns the number of comparisons made. The function also includes an early exit once the list is sorted for better results.

### Usage Instructions
- Use **`itertools.permutations()** to generate al the permutations of the list `[1, 2, 3, 4, 5]`.
- And for each permutation, call the `bubble_sort_with_count()' function.
- The permutation and the comparison count are then printed out.
---

## Usage Instructions
- Ensure Python 3 is installed on the system.
- Open tasks.ipynb in Jupyter Notebook.
- Run each task cell one by one to see the implementation and output.
- Files such as `words_alpha.txt` must be in the project directory when running task 6.

## Installation
Manually install matplotlib nltk
```bash
pip install matplotlib nltk
```
## Cloning the Repository
To clone the repository from GitHub, run the following command in the terminal
```bash
   git clone https://github.com/JakubSzkup1/computational_theory.git
   cd computational_theory
```
## Dependencies
- Python 3.x
- `matplotlib` - used for the complexity function plot.
- `nltk` - used for verifying English words with WordNet.
- `hashlib`, `math`, `itertools` - built-in libraries used across the tasks.

## References by Task
## Task 1: Binary Representations
1. [Python Bitwise Operators – Official Docs](https://docs.python.org/3/library/stdtypes.html#bitwise-operations-on-integer-types)
2. [GeeksforGeeks: Bitwise Operators in Python](https://www.geeksforgeeks.org/python-bitwise-operators/)
3. [Real Python: Bitwise Operators](https://realpython.com/python-bitwise-operators/)

## Task 2: Hash Functions
1. Kernighan, B. W., & Ritchie, D. M. (1988). *The C Programming Language* (2nd ed.). Prentice Hall.
2. [Python `ord()` Function – Official Docs](https://docs.python.org/3/library/functions.html#ord)

## Task 3: SHA-256 Padding
1. [FIPS PUB 180-4: Secure Hash Standard (SHS)](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)
2. [Wikipedia: SHA-2](https://en.wikipedia.org/wiki/SHA-2)
3. [GeeksforGeeks: What is SHA256?](https://www.geeksforgeeks.org/sha256-hash-in-python/)

## Task 4: Prime Numbers
1. [Fundamental Theorem of Arithmetic – Wikipedia](https://en.wikipedia.org/wiki/Fundamental_theorem_of_arithmetic)
2. [Sieve of Eratosthenes – Wikipedia](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
3. [GeeksforGeeks: Sieve of Eratosthenes in Python](https://www.geeksforgeeks.org/python-program-for-sieve-of-eratosthenes/)

## Task 5: Roots
1. [Square Root – Wikipedia](https://en.wikipedia.org/wiki/Square_root)
2. [Binary Number – Wikipedia](https://en.wikipedia.org/wiki/Binary_number)
3. [Python Floating Point Arithmetic – Python Docs](https://docs.python.org/3/tutorial/floatingpoint.html)

## Task 6: Proof of Work
1. [FIPS PUB 180-4, Secure Hash Standard – NIST](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf)
2. [SHA-256 – Wikipedia](https://en.wikipedia.org/wiki/SHA-2)
3. [Proof of Work – Bitcoin Wiki](https://en.bitcoin.it/wiki/Proof_of_work)
4. [English Word List – dwyl/english-words GitHub Repository](https://github.com/dwyl/english-words)
5. [WordNet – NLTK](https://www.nltk.org/howto/wordnet.html)

## Task 7: Turing Machine
1. Turing, A. M. (1937). *On Computable Numbers, with an Application to the Entscheidungsproblem*.
2. Sipser, M. (2012). *Introduction to the Theory of Computation* (3rd ed.). Cengage Learning.
3. [Wikipedia: Turing Machine](https://en.wikipedia.org/wiki/Turing_machine)

## Task 8: Computational Complexity – Bubble Sort
1. Sipser, M. (2012). *Introduction to the Theory of Computation* (3rd ed.). Cengage Learning.
2. Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press.
3. [GeeksforGeeks: Bubble Sort](https://www.geeksforgeeks.org/bubble-sort/)
4. [Programiz: Bubble Sort in Python](https://www.programiz.com/dsa/bubble-sort)





