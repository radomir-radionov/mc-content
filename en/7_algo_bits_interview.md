# Bit Manipulation — Interview

Interview answers in this file are filled.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Bit Manipulation — Interview](./7_algo_bits_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Bit Manipulation](https://devinterview.io/questions/data-structures-and-algorithms/bit-manipulation-interview-questions/).

### Bit Manipulation Fundamentals

<details>
<summary>What is a Bit?</summary>

### What is a Bit?

**Definition:**

A bit is one binary digit, the smallest unit of information in a digital computer. It is either 0 or 1.

**How it works:**

- The name is short for binary digit. As a Boolean it is false or true.
- Place values are powers of two, not ten. The bit at position `i`, counting from 0 at the right, is worth `2^i`.
- Four bits are a nibble: 16 patterns, one hex digit from `0` to `F`. Eight bits are a byte: 256 patterns, 0 through 255.
- The decimal 5 is the byte `00000101`, because `4 + 1 = 5`.
- Bitwise operators act on these bits directly. AND, OR, XOR, NOT, and shifts are single ALU operations on a machine word.
- A signed integer is usually two’s complement. A fixed 64-bit signed value spans `-2^63` through `2^63 − 1`. A language with arbitrary-precision integers, such as Python, grows the bit string instead of overflowing the word.
- The machine word is the width the CPU handles in one register, 64 bits on current general-purpose chips. Wider SIMD registers apply the same bitwise idea to several words at once.

**Examples:**

`42 & 12` is `00101010` AND `00001100`, which is `00001000`, or 8.
</details>

<details>
<summary>What is a Byte?</summary>

### What is a Byte?

**Definition:**

A byte is a group of 8 bits, also called an octet. It holds 256 distinct patterns, the unsigned values 0 through 255.

**How it works:**

- Number the bits from 0 at the right, the least significant bit, up to 7 at the left, the most significant bit. Bit `i` contributes `b_i * 2^i`.
- The place values are 1, 2, 4, 8, 16, 32, 64, and 128. All eight bits set is 255.
- On ordinary architectures the byte is the smallest addressable unit. Larger integers are sequences of bytes, interpreted with an endianness.
- `int(bit_string, 2)` parses a binary text. A left-shift reconstruction does the same without a parser: start at 0, and for each next bit do `(value << 1) | bit`.
- Raw bytes are a different object from a binary string. `int.from_bytes(b"\xff", "big")` reads the all-ones byte as 255.
- SIMD code often moves 16, 32, or 64 bytes in one instruction. The byte is still the unit those vectors are made of.

**Examples:**

```text
value = 0
for bit in [1, 0, 1, 1, 0, 0, 0, 0]:  # high bit first
    value = (value << 1) | bit
# value == 176
```
</details>

<details>
<summary>Explain what is a Bitwise Operation.</summary>

### Explain what is a Bitwise Operation.

**Definition:**

A bitwise operation combines the individual bits of one or two integers. The CPU’s arithmetic logic unit does it in a single instruction on a machine word.

**How it works:**

- AND (`&`) is 1 only where both bits are 1. It masks fields out. `5 & 3` is `0101 & 0011 = 0001`.
- OR (`|`) is 1 where either bit is 1. It sets bits. `5 | 3` is `0111`, which is 7.
- XOR (`^`) is 1 where the bits differ. It toggles and checks parity. `5 ^ 3` is `0110`, which is 6.
- NOT (`~`) flips every bit. In two’s complement, `~x` equals `-(x + 1)`, so `~5` is `-6`.
- A left shift `x << k` moves bits toward the high end and fills zeros, multiplying by `2^k` when nothing shifts out. `5 << 2` is 20.
- An arithmetic right shift `x >> k` moves bits toward the low end. On a nonnegative value it is floor division by `2^k`. `5 >> 2` is 1.
- A logical right shift (`>>>` in Java and JavaScript) fills the high end with zeros. Python has no `>>>`; mask to the width you care about, then shift.
- Negating in two’s complement is “flip the bits and add 1”. The population count is the number of 1-bits; Python exposes it as `int.bit_count()`.
- Typical uses: pack flags into one word, bloom-filter membership, permission bits, and splitting a 32-bit color into 8-bit channels.

**Examples:**

```text
READ, WRITE, EXEC = 1, 2, 4
perms = READ | WRITE          # 0b011
can_exec = perms & EXEC       # 0
perms ^= EXEC                 # toggle execute -> 0b111
```
</details>

<details>
<summary>What are some real-world applications of Bitwise Operators?</summary>

### What are some real-world applications of Bitwise Operators?

**Definition:**

Bitwise operators pack, mask, and mix bits inside a word. They show up wherever a program talks to a compact binary layout instead of a separate variable per flag.

**How it works:**

- Compression appends variable-length codes, as in Huffman coding, by shifting bits into a byte stream. Bit-packing stores several small fields, such as 10-bit channels, in one integer so they share a cache line.
- Symmetric ciphers such as AES and ChaCha20 mix state with XOR and rotates. Checksums such as CRC use XOR and shifts to detect a flipped bit.
- A subnet check is `ip & mask`. The mask keeps the network prefix and clears the host bits.
- Device registers are updated in place: `reg |= 1 << k` sets a bit, `reg &= ~(1 << k)` clears it, `reg ^= 1 << k` toggles it.
- `n > 0 and (n & (n - 1)) == 0` tests a power of two. A bitset stores a Boolean array in one bit per flag, about eight times denser than an array of bytes.
- Graphics code uses shifts instead of division by a power of two, and uses masks to isolate color channels.
- A parity bit or `bit_count() & 1` detects a single-bit error. A Bloom filter ORs hash indexes into a bit array.

**Examples:**

```text
READ, WRITE = 1 << 0, 1 << 1
state = READ | WRITE
can_write = bool(state & WRITE)
state ^= (1 << 3)             # toggle an encrypted flag
```
</details>

<details>
<summary>What is a bitwise AND operation and how can it be used to check if a number is odd or even?</summary>

### What is a bitwise AND operation and how can it be used to check if a number is odd or even?

**Definition:**

Bitwise AND writes a 1 only in positions where both inputs have a 1. AND with 1 isolates the least significant bit, which is the parity of the integer.

**How it works:**

- Every higher bit is a positive power of two, so it is even. The bit worth `2^0` is the only one that can make the number odd.
- `n & 1` is 0 when `n` is even and 1 when `n` is odd.
- The same operator is a mask. `n & mask` clears every bit that is 0 in the mask and keeps the bits that are 1 in both.
- On a fixed-width word the operation is one instruction. A compiler often rewrites `n % 2` into this test when the divisor is the constant 2.
- It works for negative two’s-complement values as well: the least significant bit is still the remainder modulo 2.

**Examples:**

```text
is_odd = (n & 1) == 1
is_even = (n & 1) == 0
```

`6 & 1` is 0. `7 & 1` is 1.
</details>

<details>
<summary>Explain the bitwise OR operation with an example.</summary>

### Explain the bitwise OR operation with an example.

**Definition:**

Bitwise OR writes a 1 in every position where at least one of the two inputs has a 1. It is the operator that turns a bit on without disturbing the others.

**How it works:**

- `0 | 0` is 0. Any pair that includes a 1 is 1. OR cannot clear a bit: a 1 in either input stays 1.
- `n | (1 << k)` forces bit `k` on and leaves every other bit of `n` as it was.
- OR of two masks is their union. A permission word is built by OR-ing the flags that should be present.
- Inclusive OR is different from XOR. OR keeps a 1 when both sides are 1. XOR would turn that position off.

**Examples:**

`0b1010 | 0b0100` is `0b1110`. In decimal, `10 | 4` is 14. `0b1010 | 0b0010` stays `0b1010`, because that bit was already set.
</details>

<details>
<summary>How does the bitwise XOR operation work, and what is it commonly used for?</summary>

### How does the bitwise XOR operation work, and what is it commonly used for?

**Definition:**

XOR writes a 1 where the two bits differ and a 0 where they match. It is its own inverse, which is why it shows up in swaps, checksums, and “find the unique element”.

**How it works:**

- `0 ^ 0` and `1 ^ 1` are 0. `0 ^ 1` and `1 ^ 0` are 1.
- `a ^ a` is 0, `a ^ 0` is `a`, and `a ^ b ^ b` is `a`. The operation is commutative and associative.
- XOR of an array in which every value appears twice cancels those pairs and leaves the value that appears once.
- Three XORs swap two variables: `a ^= b; b ^= a; a ^= b`. It fails if `a` and `b` are the same object.
- Inside an adder, XOR is the sum of two bits with the carry removed. The carry is the AND, shifted left.
- Flipping any input bit flips the running XOR, so a one-bit change is visible in a parity checksum.
- `n ^ (1 << k)` toggles bit `k` and leaves the rest of the word alone.

**Examples:**

`5 ^ 3` is `0101 ^ 0011 = 0110`, which is 6. XOR that with 3 again and you are back at 5.
</details>

<details>
<summary>Demonstrate how to set, toggle, and clear a specific bit in a number using bitwise operators.</summary>

### Demonstrate how to set, toggle, and clear a specific bit in a number using bitwise operators.

**Definition:**

Build a one-bit mask `1 << k` and combine it with the word. OR sets the bit, XOR toggles it, and AND with the inverted mask clears it.

**How it works:**

- Number bits from 0 at the least significant end. `mask = 1 << k` has only bit `k` set.
- Set: `n | mask`. A 1 in the mask forces that position on.
- Toggle: `n ^ mask`. Positions where the mask is 0 stay unchanged, and bit `k` flips.
- Clear: `n & ~mask`. The complement has a 0 only at bit `k`, so AND keeps every other bit and forces bit `k` off.
- Test: `(n & mask) != 0`.
- On a fixed-width signed word, `~mask` has the high bits set. That is what you want for the clear. In a language with infinite sign bits, mask the complement to the width you are using.

**Examples:**

```text
mask = 1 << 2          # 0b100
n = 0b1001
set    = n | mask      # 0b1101
toggle = n ^ mask      # 0b1101
clear  = set & ~mask   # 0b1001
```
</details>

<details>
<summary>What is bit masking, and how would you create a mask to isolate the nth bit?</summary>

### What is bit masking, and how would you create a mask to isolate the nth bit?

**Definition:**

A mask is an integer whose 1-bits mark the positions you want to read or change. Isolating bit `n` means AND with a mask that has only that bit set.

**How it works:**

- `mask = 1 << n`. Then `value & mask` is either 0 or `mask`.
- Shift the result right by `n` if you want the bit as 0 or 1: `(value >> n) & 1`.
- A field from bit `lo` through bit `hi` uses a run of ones, `((1 << (hi - lo + 1)) - 1) << lo`, then AND.
- Masks compose. OR unions them, AND intersects them, and XOR flips exactly the marked bits.
- Clearing uses the complement of the mask. Setting uses OR. The mask itself does not include the data bits.

**Examples:**

```text
isolate bit 3 of 0b11010:
  0b11010 & (1 << 3)     # 0b01000
  (0b11010 >> 3) & 1     # 1
```
</details>

<details>
<summary>Explain how left and right shifts (`<<` and `>>`) work in bit manipulation.</summary>

### Explain how left and right shifts (`<<` and `>>`) work in bit manipulation.

**Definition:**

A shift moves every bit the same number of places. Bits that leave the word disappear, and the vacated end is filled with a pad bit.

**How it works:**

- `n << k` moves bits toward the high end and fills the low end with zeros. When the result still fits, that multiplies by `2^k`. `5 << 2` is `10100`, which is 20.
- A logical right shift moves bits toward the low end and fills the high end with zeros. On an unsigned value it is floor division by `2^k`.
- An arithmetic right shift copies the sign bit into the vacated positions, so a negative two’s-complement value stays negative. For a nonnegative value it matches the logical shift. `5 >> 2` is 1.
- The shift count is taken modulo the word width on some architectures and is undefined for a count outside `0 .. width-1` in others. Reduce `k` yourself if the language does not.
- Shifting into the sign bit, or shifting a 1 out of a fixed-width signed word, is where overflow shows up. Arbitrary-precision integers just grow.

**Examples:**

`0b001101 << 1` is `0b011010`. `0b001101 >> 1` is `0b000110`. The bit that fell off the right is gone; a rotation would have kept it.
</details>

<details>
<summary>What is the difference between `>>` and `>>>` operators?</summary>

### What is the difference between `>>` and `>>>` operators?

**Definition:**

`>>` is an arithmetic right shift: the vacated high bits are filled with a copy of the sign bit. `>>>` is a logical right shift: those bits are filled with zeros.

**How it works:**

- On a nonnegative value the two operators match, because the sign bit is already 0.
- On a negative two’s-complement value, `>>` keeps the result negative. `>>>` eventually produces a nonnegative bit pattern because zeros enter from the left.
- Java defines `>>>` on `int` and `long`. JavaScript converts the operand to a 32-bit integer first: signed for `>>`, unsigned for `>>>`.
- Python has no `>>>`. The same effect on a width-`w` pattern is `(n & ((1 << w) - 1)) >> k`.
- Use `>>>` or an unsigned mask when you are sliding a bit field and must not smear a sign bit into the data. Use `>>` when you mean floor division of a signed integer by a power of two.

**Examples:**

In a 32-bit word, `-8 >> 1` stays negative (`-4`). `-8 >>> 1` is a large positive value, because the sign bit moved right and a zero took its place.
</details>

<details>
<summary>Explain how to perform left and right bit rotations.</summary>

### Explain how to perform left and right bit rotations.

**Definition:**

A rotation moves bits the way a shift does, then feeds the bits that fell off one end back in at the other end. The word keeps the same number of 1-bits.

**How it works:**

- For a word of width `w`, rotate left by `k` with `((n << k) | (n >> (w - k)))` masked to `w` bits. Take `k` modulo `w` first.
- The right shift in that formula must be logical. An arithmetic shift would copy a sign bit instead of the bits that wrapped.
- Rotate right by `k` is a left rotate by `w - k`, or the same formula with the distances swapped.
- A plain shift destroys bits and fills zeros. A rotation is reversible: rotating back by the same distance restores the word.
- Hashes and ciphers use rotations because they mix high bits into low bits without the zero-fill of a shift.

**Examples:**

```text
def rotl(n, k, w=8):
    k %= w
    n &= (1 << w) - 1
    return ((n << k) | (n >> (w - k))) & ((1 << w) - 1)
```

`rotl(0b10000001, 1)` is `0b00000011`.
</details>

### Bitwise Problem Solving and Algorithms

<details>
<summary>Write a function that counts the number of set bits (1s) in an integer.</summary>

### Write a function that counts the number of set bits (1s) in an integer.

**Definition:**

The population count is the number of 1-bits. Kernighan’s loop clears one set bit per iteration, so it runs once per 1-bit rather than once per bit of width.

**How it works:**

- `n & (n - 1)` turns off the lowest set bit. Subtracting one flips that bit and every zero below it; the AND keeps the higher 1-bits.
- Repeat until `n` is 0 and count the iterations.
- A fixed-width word can also use a hardware popcount, or a parallel sum that adds adjacent bit groups with a few masks and shifts. That parallel method is a constant number of operations on a 32-bit or 64-bit word.
- Python’s `int.bit_count()` is the library form. On an arbitrary-precision integer the cost follows the number of limbs, not a single instruction.
- The result is an integer from 0 up to the width, also called the Hamming weight.

**Examples:**

```text
def popcount(n):
    count = 0
    while n:
        n &= n - 1
        count += 1
    return count
```

`popcount(0b10110)` is 3.
</details>

<details>
<summary>Determine if a number is a power of two using bit manipulation.</summary>

### Determine if a number is a power of two using bit manipulation.

**Definition:**

A positive power of two has exactly one bit set. `n > 0 and n & (n - 1) == 0` detects that pattern.

**How it works:**

- `8` is `1000`. `7` is `0111`. Their AND is 0.
- Any other positive pattern has at least two 1-bits, so clearing the lowest one still leaves a 1, and the AND is not 0.
- Zero is not a positive power of two, and `0 & -1` would be 0, so the `n > 0` test is required.
- Negative values are outside this test. In two’s complement they do not have a single positive power-of-two pattern.
- The test is one instruction after the comparison. A loop that divides by two also works and does Θ(log n) steps.

**Examples:**

```text
def is_pow2(n):
    return n > 0 and (n & (n - 1)) == 0
```

`is_pow2(16)` is true. `is_pow2(12)` is false. `is_pow2(0)` is false.
</details>

<details>
<summary>Design a function that adds two numbers without using the ‘+’ operator.</summary>

### Design a function that adds two numbers without using the ‘+’ operator.

**Definition:**

XOR is the sum of two bits with the carry thrown away, and a left-shifted AND is that carry. Repeat until the carry word is zero.

**How it works:**

- `sum_bits = a ^ b` adds each column and drops the carry.
- `carry = (a & b) << 1` is a 1 in the next column wherever both bits were 1.
- Replace `a` with `sum_bits` and `b` with `carry`, and repeat. When `b` is 0 there is nothing left to carry, and `a` is the total.
- This is a ripple-carry adder written as a loop. On a fixed-width word the carry shifts out within that width, so the loop ends.
- Subtraction is the same routine on `a` and the two’s-complement negation of `b` (`~b + 1`), which is allowed if the ban is only on the `+` source token you can build the increment from the carry loop itself.
- Overflow on a fixed-width signed word is the carry into the sign bit, the same as with ordinary addition.

**Examples:**

```text
def add(a, b):
    while b:
        carry = (a & b) << 1
        a = a ^ b
        b = carry
    return a
```

`add(5, 3)` walks the carry until it returns 8.
</details>

<details>
<summary>Implement a function that swaps the values of two variables without using extra space.</summary>

### Implement a function that swaps the values of two variables without using extra space.

**Definition:**

XOR swaps two distinct storage locations with three assignments and no temporary word. GeeksforGeeks also shows an add-and-subtract swap; the XOR form is the bit version.

**How it works:**

- `a ^= b` stores the mixed bits. `b ^= a` recovers the old `a`. `a ^= b` recovers the old `b`. The tactics page times this at O(1) time and O(1) auxiliary space.
- XOR is its own inverse, and `x ^ x` is 0, so the mixed word still contains both originals.
- The three steps must name two different locations. XORing one word with itself clears it.
- The arithmetic swap `a = a + b; b = a - b; a = a - b` also uses no third variable, and on a fixed-width word the sum can overflow. XOR does not add.

**Examples:**

```text
def xor_swap(a, b):
    a ^= b
    b ^= a
    a ^= b
    return a, b
```

`xor_swap(5, 7)` returns `(7, 5)`.

**Sources:**

- [GeeksforGeeks — Swap two numbers](https://www.geeksforgeeks.org/dsa/swap-two-numbers-without-using-temporary-variable/)
- [GeeksforGeeks — Bits manipulation tactics](https://www.geeksforgeeks.org/dsa/bits-manipulation-important-tactics/)
</details>

<details>
<summary>Find the only non-repeating element in an array where each element repeats twice, using bitwise operations.</summary>

### Find the only non-repeating element in an array where each element repeats twice, using bitwise operations.

**Definition:**

XOR the whole array. Pairs cancel, and the value that appears once remains. GeeksforGeeks calls this the expected approach: O(n) time and O(1) space.

**How it works:**

- `x ^ x` is 0, and `x ^ 0` is `x`. Order does not matter because XOR is associative and commutative.
- Start from 0 and fold `res ^= arr[i]` once per element.
- A hash map is also O(n) time but O(n) space. A nested count is O(n²) time and O(1) space.
- The XOR fold assumes every other value appears an even number of times, here twice.

**Examples:**

```text
def find_unique(arr):
    res = 0
    for num in arr:
        res ^= num
    return res
```

`find_unique([2, 2, 5, 5, 20, 30, 30])` is 20.

**Sources:**

- [GeeksforGeeks — Unique Number I](https://www.geeksforgeeks.org/dsa/find-element-appears-array-every-element-appears-twice/)
</details>

<details>
<summary>Given a list of numbers where every number appears three times except for one, find the single non-repeated number.</summary>

### Given a list of numbers where every number appears three times except for one, find the single non-repeated number.

**Definition:**

Count each bit position modulo 3. A bit that appears three times sums to a multiple of 3 and disappears; the singleton’s bits remain. GeeksforGeeks times the bit and bitmask forms at O(n) time and O(1) space.

**How it works:**

- For a fixed word width, walk each bit once. The outer width is constant, so 32 or 64 passes are still O(n).
- The one-pass bitmask keeps `ones` and `twos`. Bits seen three times land in both and are cleared with `mask = ~(ones & twos)`.
- A frequency map is O(n) time and O(n) space. A nested count is O(n²).
- On an arbitrary-precision integer, mask the working words to the width you are counting, or `~` fills every higher bit.

**Examples:**

```text
def single_number(arr):
    ones = twos = 0
    for num in arr:
        twos |= ones & num
        ones ^= num
        mask = ~(ones & twos)
        ones &= mask
        twos &= mask
    return ones
```

`single_number([1, 10, 1, 1])` is 10.

**Sources:**

- [GeeksforGeeks — Unique Number III](https://www.geeksforgeeks.org/dsa/find-the-element-that-appears-once/)
</details>

<details>
<summary>Implement a bit manipulation algorithm to find and clear the lowest set bit in a number.</summary>

### Implement a bit manipulation algorithm to find and clear the lowest set bit in a number.

**Definition:**

The lowest set bit is the mask `n & -n` in two’s complement. Clearing it is `n & (n - 1)`. GeeksforGeeks times both at O(1) time and O(1) space; a scan from the right is O(log n).

**How it works:**

- Subtracting 1 flips the lowest 1 and every 0 below it. AND with `n` drops that 1 and keeps the higher bits. For 12 (`1100`) the result is 8 (`1000`).
- `n & -n` isolates that same bit, because `-n` is `~n + 1`. For 12 the isolated bit is 4.
- Zero has no set bit. Both formulas return 0.
- Clearing is one expression. Finding the index of that bit is a separate log of the isolated mask.

**Examples:**

```text
def lowest_set(n):
    return n & -n

def clear_lowest(n):
    return n & (n - 1)
```

`lowest_set(12)` is 4. `clear_lowest(12)` is 8. `clear_lowest(7)` is 6.

**Sources:**

- [GeeksforGeeks — Turn off the rightmost set bit](https://www.geeksforgeeks.org/dsa/turn-off-the-rightmost-set-bit/)
</details>

<details>
<summary>Write a function that would generate the next higher number with the same number of set bits as the given positive integer.</summary>

### Write a function that would generate the next higher number with the same number of set bits as the given positive integer.

**Definition:**

Move the rightmost block of 1-bits one place left and pack the leftover 1-bits to the low end. GeeksforGeeks times this bit recipe at O(1) time and O(1) auxiliary space.

**How it works:**

- `rightOne = x & -x` isolates the lowest 1.
- `next = x + rightOne` clears that trailing block and sets the 0 just above it.
- `rightOnesPattern = (x ^ next) // rightOne >> 2` is the leftover 1-bits, shifted to the bottom. The extra shift by 2 corrects the factor of two introduced by the addition.
- OR those bits back onto `next`. For 156 (`10011100`) the result is 163 (`10100011`). For 12 (`1100`) the result is 17 (`10001`).
- Zero stays zero. A walking search that increments until the popcount matches is O(log n) bit inspections per candidate, not this constant recipe.

**Examples:**

```text
def next_same_popcount(x):
    if x <= 0:
        return 0
    right_one = x & -x
    higher = x + right_one
    pattern = (x ^ higher) // right_one
    pattern >>= 2
    return higher | pattern
```

`next_same_popcount(156)` is 163. `next_same_popcount(12)` is 17.

**Sources:**

- [GeeksforGeeks — Next higher number with same number of set bits](https://www.geeksforgeeks.org/dsa/next-higher-number-with-same-number-of-set-bits/)
</details>

<details>
<summary>Reverse the bits of an unsigned integer.</summary>

### Reverse the bits of an unsigned integer.

**Definition:**

Walk each bit of a fixed width and place it at the mirrored index. GeeksforGeeks times that loop at O(log n) time and O(1) auxiliary space, because a number n has about log n bits. A lookup table for a known width is O(1).

**How it works:**

- For a 32-bit word, bit `i` of the input becomes bit `31 - i` of the output.
- A second form shifts the reversed word left and ORs in the next low bit, then shifts the leftover zeros into place after the input is consumed.
- The width must be fixed. Reversing only the significant bits, without leading zeros, is a different problem and is what a binary-palindrome check usually wants.
- `reverse_bits(1)` on 32 bits is `1 << 31`.

**Examples:**

```text
def reverse_bits(num, width=32):
    out = 0
    for i in range(width):
        if num & (1 << i):
            out |= 1 << (width - 1 - i)
    return out
```

`reverse_bits(1)` is `2147483648`.

**Sources:**

- [GeeksforGeeks — Reverse bits of a number](https://www.geeksforgeeks.org/dsa/write-an-efficient-c-program-to-reverse-bits-of-a-number/)
</details>

<details>
<summary>Calculate the absolute value of an integer using bit manipulation.</summary>

### Calculate the absolute value of an integer using bit manipulation.

**Definition:**

A negative two’s-complement word is inverted and incremented. The sign bit, smeared by an arithmetic right shift, selects that transform without a branch. GeeksforGeeks times it at O(1) time and O(1) auxiliary space.

**How it works:**

- `mask = n >> 31` on a 32-bit signed word is all ones when `n` is negative and all zeros when it is not.
- `(n ^ mask) - mask` is `n` when the mask is 0, and `~n + 1` when the mask is all ones.
- `(n + mask) ^ mask` is the other form of the same idea.
- The most negative 32-bit value has no positive counterpart in that width.
- Python integers do not have a 32-bit sign bit, so the shift width has to be applied to a masked word, not to an unbounded `int`.

**Examples:**

```text
def abs32(n):
    n &= 0xFFFFFFFF
    if n & 0x80000000:
        n -= 0x100000000
    mask = -1 if n < 0 else 0
    return (n ^ mask) - mask
```

`abs32(-6)` is 6. `abs32(6)` is 6.

**Sources:**

- [GeeksforGeeks — Absolute value without branching](https://www.geeksforgeeks.org/dsa/compute-the-integer-absolute-value-abs-without-branching/)
</details>

<details>
<summary>Multiply and divide a number by two using bit manipulation.</summary>

### Multiply and divide a number by two using bit manipulation.

**Definition:**

A left shift by one multiplies by two. A right shift by one divides by two. GeeksforGeeks shows both as one-bit tricks: `x <<= 1` and `x >>= 1`.

**How it works:**

- Each bit is a power of two, so moving every bit one place left doubles its weight. `18` is `00010010`; `18 << 1` is `36`.
- A right shift halves each weight. `18 >> 1` is `9`.
- On a fixed-width word the top bit of a left shift falls off, which is overflow, and an arithmetic right shift copies the sign bit.
- One shift is a constant amount of work, the same class as the other single-mask tricks on that page.

**Examples:**

```text
def mul2(x):
    return x << 1

def div2(x):
    return x >> 1
```

`mul2(18)` is 36. `div2(18)` is 9.

**Sources:**

- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
</details>

<details>
<summary>Determine the log base 2 of an integer using bitwise operations.</summary>

### Determine the log base 2 of an integer using bitwise operations.

**Definition:**

Floor of log base 2 is the index of the highest set bit. GeeksforGeeks counts right shifts until the word becomes 0. For a fixed 32-bit word the same page’s MSB smear is O(1) time and O(1) space.

**How it works:**

- `res = 0; while x >>= 1: res += 1` counts how many times the value can be halved before it vanishes. That is floor(log2 x) for x > 0.
- The loop follows the number of significant bits, so it is proportional to log x.
- A 32-bit smear `n |= n >> 1`, then 2, 4, 8, and 16, fills every bit below the MSB. Adding one and shifting back leaves only that MSB. The tactics page times this fixed-width form at O(1).
- Zero has no logarithm. The shift loop on 0 returns 0 and must be rejected separately.

**Examples:**

```text
def floor_log2(x):
    res = 0
    while x >> 1:
        x >>= 1
        res += 1
    return res
```

`floor_log2(18)` is 4, because 16 is the greatest power of two not above 18.

**Sources:**

- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
- [GeeksforGeeks — Bits manipulation tactics](https://www.geeksforgeeks.org/dsa/bits-manipulation-important-tactics/)
</details>

### Advanced Bit Manipulation Techniques

<details>
<summary>How can you detect and handle integer overflow and underflow while using bit manipulation for arithmetic operations?</summary>

### How can you detect and handle integer overflow and underflow while using bit manipulation for arithmetic operations?

**Definition:**

On a two’s-complement word, signed addition overflows when both addends share a sign and the wrapped sum does not. That is the same sign-bit test GeeksforGeeks uses for opposite signs, applied to each addend against the sum. The test is O(1) time and O(1) space.

**How it works:**

- Compute the wrapped sum in the word width, or detect the carry out of the top bit for an unsigned add.
- Signed overflow: `(a ^ s) & (b ^ s)` has the sign bit set. `a` and `b` then differ in sign from `s`.
- Underflow of `a - b` is overflow of `a + (-b)`. Negating the most negative value itself overflows.
- Handling is a wider accumulator, a saturating clamp, or an error. A language with unbounded integers, such as Python, does not wrap, so the check belongs on a masked word.
- A left shift that drops a 1 is the overflow of multiplying by a power of two.

**Examples:**

```text
def signed_add_overflows(a, b, width=32):
    mask = (1 << width) - 1
    sign = 1 << (width - 1)
    s = (a + b) & mask
    return ((a ^ s) & (b ^ s) & sign) != 0
```

`signed_add_overflows(2_000_000_000, 2_000_000_000)` is true on 32 bits.

**Sources:**

- [GeeksforGeeks — Opposite signs](https://www.geeksforgeeks.org/dsa/detect-if-two-integers-have-opposite-signs/)
</details>

<details>
<summary>Explain the significance of setting flags and checking bits in the implementation of a bitmap or bitset.</summary>

### Explain the significance of setting flags and checking bits in the implementation of a bitmap or bitset.

**Definition:**

A bitmap stores one boolean per bit, so eight flags fit in a byte and a machine word holds a whole row of them. GeeksforGeeks turns a chosen bit on with OR, off with AND-NOT, and tests it with AND.

**How it works:**

- Bit `k` (counting from 1 at the right in that article) is the mask `1 << (k - 1)`.
- Set: `n | mask`. Clear: `n & ~mask`. Test: `(n & mask) != 0`. Toggle: `n ^ mask`.
- Each of those is one bitwise expression, the same constant-work class as the other one-liners on the bit-tricks page.
- The point of the packing is density. A byte of flags is one addressable unit instead of eight booleans, and a scan of a bitset walks words, not one flag per byte.
- The same masks are how a bitset answers “is index i present?” without a hash table.

**Examples:**

```text
def set_bit(n, k):
    return n | (1 << k)

def clear_bit(n, k):
    return n & ~(1 << k)

def test_bit(n, k):
    return (n & (1 << k)) != 0
```

`test_bit(set_bit(0, 0) | set_bit(0, 3), 3)` is true. Bits 0 and 3 are the flags.

**Sources:**

- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
</details>

<details>
<summary>How can bit manipulation be used to perform efficient matrix multiplication of binary matrices?</summary>

### How can bit manipulation be used to perform efficient matrix multiplication of binary matrices?

**Definition:**

A boolean product replaces multiply-add with AND and OR. Packing a row into machine words turns a run of those ANDs into one word operation. The unpacked triple loop has the same cubic shape GeeksforGeeks times at O(V³) for a V×V Floyd–Warshall matrix.

**How it works:**

- Entry `(i, j)` is the OR over `k` of `A[i][k] AND B[k][j]`. Over GF(2) the OR becomes XOR and the product is parity.
- Store row `i` of A as a bitset. For each `j`, walk `k` a word at a time: AND the word of A with a word assembled from column `j` of B, then OR (or XOR) the bits of that word into the accumulator.
- The word width `w` shortens the `k` loop by about `w`. The triple loop is still Θ(n³ / w) word operations for n×n matrices, which is the cubic loop divided by the constant width.
- This pays off when the matrix is dense and the entries are only 0 and 1. A sparse graph still wants a sparse product.

**Examples:**

```text
def bool_mul(a, b):
    n = len(a)
    out = [[0] * n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if a[i][k]:
                for j in range(n):
                    out[i][j] |= b[k][j]
    return out
```

Packing row `b[k]` into an integer replaces the inner `j` loop with one OR of that word.

**Sources:**

- [GeeksforGeeks — Floyd–Warshall](https://www.geeksforgeeks.org/dsa/floyd-warshall-algorithm-dp-16/)
- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
</details>

<details>
<summary>Design an algorithm to perform endian conversion (big endian to little endian and vice versa) using bit manipulation.</summary>

### Design an algorithm to perform endian conversion (big endian to little endian and vice versa) using bit manipulation.

**Definition:**

Endianness is the order of bytes in a multi-byte word. Big-endian stores the most significant byte at the lowest address; little-endian stores the least significant byte there. Swapping the bytes reverses the order. GeeksforGeeks uses `0x12345678` as the example: big-endian memory is `12 34 56 78`, little-endian memory is `78 56 34 12`.

**How it works:**

- A 32-bit swap is four masks and shifts. The low byte moves to the high end, and the high byte moves to the low end.
- The same function converts either way: swapping twice returns the original word.
- Network order is big-endian, and most PCs are little-endian, so a socket path converts multi-byte fields at the boundary.
- The transform is a fixed number of shifts on a fixed-width word, so it is constant work per word. A buffer of n bytes is Θ(n).

**Examples:**

```text
def bswap32(n):
    return (((n & 0xFF) << 24)
            | ((n & 0xFF00) << 8)
            | ((n >> 8) & 0xFF00)
            | ((n >> 24) & 0xFF))
```

`bswap32(0x12345678)` is `0x78563412`.

**Sources:**

- [GeeksforGeeks — Big-endian and little-endian](https://www.geeksforgeeks.org/dsa/little-and-big-endian-mystery/)
</details>

### Bit Manipulation in Computing and Data Processing

<details>
<summary>Give an example of how to use bitwise operations to implement a simple compression algorithm.</summary>

### Give an example of how to use bitwise operations to implement a simple compression algorithm.

**Definition:**

Eight boolean flags, or eight symbols from a two-symbol alphabet, pack into one byte with shifts and OR. The bit-tricks page is the packing primitive: a left shift places a bit, OR merges it, AND reads it back.

**How it works:**

- Unpacked, each flag may occupy a whole byte. Packed, the same eight flags occupy one byte, an 8-to-1 reduction for that alphabet.
- Encode walks the symbols and ORs `bit << i` into an accumulator. Decode tests bit `i`.
- A run-length step can sit on top: store a bit value and a small count in the next bits when a run is long. The bit copy itself is still the shift-and-mask loop.
- Work is Θ(n) for n symbols, one shift per symbol. This is lossless only for the alphabet you packed; it is not a general compressor.

**Examples:**

```text
def pack8(flags):
    out = 0
    for i, bit in enumerate(flags[:8]):
        if bit:
            out |= 1 << i
    return out

def unpack8(word):
    return [((word >> i) & 1) for i in range(8)]
```

`pack8([1, 0, 1, 1, 0, 0, 0, 0])` is `0b00001101`. `unpack8` of that word restores the eight flags.

**Sources:**

- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
</details>

<details>
<summary>Can bitwise operations be used to efficiently encode and decode data? Illustrate with an example.</summary>

### Can bitwise operations be used to efficiently encode and decode data? Illustrate with an example.

**Definition:**

Yes. A field that does not fill a byte is stored in a slice of bits: shift it into place and mask it back out. The bit-tricks page is the read and write. Base64 is the same idea at a 6-bit width, which MDN states explicitly: each digit is 6 bits, and three bytes become four digits.

**How it works:**

- Encode: `(word << shift) & mask` drops a field into a packed integer.
- Decode: `(packed >> shift) & ((1 << width) - 1)` pulls it out.
- Three bytes are 24 bits, which is four 6-bit digits. That is why a Base64 text is about a third larger than the bytes it carries.
- The pack and unpack of a fixed number of fields is O(1). A buffer of n bytes is Θ(n).

**Examples:**

```text
def pack_fields(a, b):
    # a in 4 bits, b in the next 4
    return (a & 0xF) | ((b & 0xF) << 4)

def unpack_fields(word):
    return word & 0xF, (word >> 4) & 0xF
```

`pack_fields(0b1010, 0b0011)` is `0b00111010`. `unpack_fields` returns `(10, 3)`.

**Sources:**

- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
- [MDN — Base64](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
</details>

<details>
<summary>How are bitwise operations used in Huffman coding or other compression algorithms?</summary>

### How are bitwise operations used in Huffman coding or other compression algorithms?

**Definition:**

A Huffman code is an optimal prefix code: frequent symbols get shorter bit strings. Wikipedia notes that building the code is linear in the number of weights when those weights are already sorted. The bits of each codeword are then packed with shifts, because codewords are not byte-aligned.

**How it works:**

- The encoder walks symbols and appends the codeword bits into a bit buffer: shift in the next bit and flush every 8 bits to a byte.
- A prefix code needs no separator. The decoder reads one bit at a time and walks the tree until it hits a leaf.
- The classic illustration on the Wikipedia page encodes “this is an example of a huffman tree” in 135 bits instead of 288 bits of 8-bit characters, when the tree is already known to the decoder.
- Huffman is optimal among codes that encode one symbol at a time. Arithmetic coding can do better when a fractional bit per symbol matters.
- Emitting the bitstream is Θ(n) for n symbols, plus the cost of building the tree.

**Examples:**

```text
def emit(bits, code):
    for bit in code:
        bits.append(bit)

def to_bytes(bits):
    out = []
    for i in range(0, len(bits), 8):
        byte = 0
        for j, bit in enumerate(bits[i:i + 8]):
            byte |= bit << j
        out.append(byte)
    return out
```

A symbol whose code is `010` contributes those three bits, not a whole byte.

**Sources:**

- [Wikipedia — Huffman coding](https://en.wikipedia.org/wiki/Huffman_coding)
</details>

<details>
<summary>Explain the role of bit manipulation in encoding and decoding data, like Base64.</summary>

### Explain the role of bit manipulation in encoding and decoding data, like Base64.

**Definition:**

Base64 maps binary bytes to ASCII by regrouping bits. MDN states the size rule: each digit is 6 bits, so three 8-bit bytes (24 bits) become four 6-bit digits. The alphabet is `A–Z`, `a–z`, `0–9`, `+`, `/`, and `=` pads the last group.

**How it works:**

- Take 24 bits, split them into four groups of 6, and index the alphabet with each group.
- A final group of 1 byte uses two digits (12 bits, of which 4 are padding zeros). Two bytes use three digits. RFC 4648, as MDN summarizes it, requires those extra bits to be zero.
- The text is typically about one third larger than the bytes.
- URL-safe Base64 uses `-` and `_` instead of `+` and `/`.
- Encoding or decoding n bytes is Θ(n). The bit work is the shift-and-mask that isolates each 6-bit group.

**Examples:**

```text
ALPH = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"

def b64_triplet(b0, b1, b2):
    n = (b0 << 16) | (b1 << 8) | b2
    return "".join(ALPH[(n >> shift) & 63] for shift in (18, 12, 6, 0))
```

Three bytes `0x01 0x02 0x03` regroup into four 6-bit indexes.

**Sources:**

- [MDN — Base64](https://developer.mozilla.org/en-US/docs/Glossary/Base64)
</details>

<details>
<summary>Discuss how bitmasking can manage the state of multiple boolean flags in a single byte.</summary>

### Discuss how bitmasking can manage the state of multiple boolean flags in a single byte.

**Definition:**

One byte holds eight independent booleans. Each flag is a distinct bit, and a mask names that bit. GeeksforGeeks sets, clears, tests, and toggles bit `k` with one expression each.

**How it works:**

- Assign meanings to bits: bit 0 read, bit 1 write, bit 2 execute.
- Combine flags with OR: `READ | WRITE` is a single byte with two bits set.
- Test with AND. Clear with AND of the complement so the other flags stay.
- A byte is the unit, so the update is a constant-time read-modify-write of that byte, not eight stores.
- The limit is the width. A ninth flag needs a wider word or a second byte.

**Examples:**

```text
READ, WRITE, EXEC = 1 << 0, 1 << 1, 1 << 2

def grant(flags, perm):
    return flags | perm

def allows(flags, perm):
    return (flags & perm) == perm
```

`allows(grant(0, READ | EXEC), EXEC)` is true. `allows` of that word for `WRITE` is false.

**Sources:**

- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
</details>

<details>
<summary>Explain the use of bit manipulation in network programming, specifically in creating and interpreting network packets.</summary>

### Explain the use of bit manipulation in network programming, specifically in creating and interpreting network packets.

**Definition:**

Multi-byte header fields cross machines, and GeeksforGeeks states that network byte order is big-endian while most PCs are little-endian. Inside a byte, flags and small integers are masks and shifts.

**How it works:**

- A 16-bit port or length is two bytes. On a little-endian host you swap before writing the wire image and swap again when reading it.
- A nibble or a flag inside the first byte is not a whole field: `(byte >> 4) & 0xF` reads the high 4 bits, `byte & 0xF` reads the low 4.
- Building a header is packing those fields into a buffer. Parsing is the inverse masks.
- Checksums and protocol bits are the same AND/OR/shift toolkit. The byte-order conversion is the endian swap, constant work per field; walking a packet of n bytes is Θ(n).

**Examples:**

```text
def store_u16_be(n):
    return bytes(((n >> 8) & 0xFF, n & 0xFF))

def load_u16_be(buf):
    return (buf[0] << 8) | buf[1]
```

`store_u16_be(0x1234)` is the bytes `12 34`. `load_u16_be` of those bytes returns `0x1234`.

**Sources:**

- [GeeksforGeeks — Big-endian and little-endian](https://www.geeksforgeeks.org/dsa/little-and-big-endian-mystery/)
- [GeeksforGeeks — Bit tricks for competitive programming](https://www.geeksforgeeks.org/dsa/bit-tricks-competitive-programming/)
</details>

### Bit Manipulation Challenges and Creative Use Cases

<details>
<summary>Implement a modular exponentiation algorithm using bit manipulation.</summary>

### Implement a modular exponentiation algorithm using bit manipulation.

**Definition:**

Binary exponentiation computes `a^n mod m` from the bits of the exponent. CP-Algorithms states the cost as O(log n) multiplications: square for every bit, and multiply into the result when the bit is set. Each product is reduced modulo m, because `(a·b) mod m` equals `((a mod m)·(b mod m)) mod m`.

**How it works:**

- `res` starts at 1. While the exponent is positive, a set low bit means `res = res * a % m`.
- Then `a = a * a % m` and the exponent shifts right. That walks `floor(log2 n) + 1` bits.
- Reducing after every multiply keeps the operands smaller than `m`, so the product is at most `(m - 1)²` before the remainder, not the full `a^n`.
- The naive loop multiplies by `a` once per unit of the exponent, which is O(n) multiplications.
- If `m` is prime and `gcd(a, m) = 1`, Fermat lets you reduce the exponent modulo `m - 1` before this loop. That is optional.

**Examples:**

```text
def modpow(a, n, m):
    a %= m
    res = 1 % m
    while n > 0:
        if n & 1:
            res = res * a % m
        a = a * a % m
        n >>= 1
    return res
```

`modpow(3, 13, 1000)` is 323, because `3^13 = 1594323`.

**Sources:**

- [CP-Algorithms — Binary exponentiation](https://cp-algorithms.com/algebra/binary-exp.html)
</details>

<details>
<summary>Find the XOR of all numbers within a given range.</summary>

### Find the XOR of all numbers within a given range.

**Definition:**

XOR from L through R is the prefix XOR through R, XORed with the prefix through L − 1. GeeksforGeeks times that pattern at O(1) time and O(1) space. A loop from L to R is O(n).

**How it works:**

- The prefix `1 ^ 2 ^ … ^ n` depends on `n % 4`: it is `n` when the remainder is 0, `1` when it is 1, `n + 1` when it is 2, and `0` when it is 3.
- Values from 1 through L − 1 appear in both prefixes, so they cancel.
- The same page’s naive loop is the definition and the check: `4 ^ 5 ^ 6 ^ 7 ^ 8` is 8, and `2 ^ 3 ^ 4` is 5.

**Examples:**

```text
def prefix_xor(n):
    if n < 0:
        return 0
    return (n, 1, n + 1, 0)[n % 4]

def range_xor(left, right):
    return prefix_xor(right) ^ prefix_xor(left - 1)
```

`range_xor(4, 8)` is 8. `range_xor(2, 4)` is 5.

**Sources:**

- [GeeksforGeeks — XOR of a range](https://www.geeksforgeeks.org/dsa/find-xor-of-numbers-from-the-range-l-r/)
</details>

<details>
<summary>Generate all possible subsets of a set using bit manipulation.</summary>

### Generate all possible subsets of a set using bit manipulation.

**Definition:**

Each integer from 0 through `2^n − 1` is one subset: bit `j` says whether element `j` is in it. GeeksforGeeks states that a set of n elements has `2^n` subsets, and this scan is O(n · 2^n) time. The bit version’s auxiliary space, apart from the output, is O(1).

**How it works:**

- Mask `i` selects element `j` when `i & (1 << j)` is set.
- There are `2^n` masks and n bit tests each, which is the stated O(n · 2^n).
- For `"abc"` the masks produce the empty string and every non-empty subset.
- Writing the subsets down takes Θ(n · 2^n) output size, so the bound is tight if you materialize them.

**Examples:**

```text
def subsets(items):
    n = len(items)
    out = []
    for mask in range(1 << n):
        out.append([items[j] for j in range(n) if mask & (1 << j)])
    return out
```

`subsets(["a", "b"])` is `[[], ["a"], ["b"], ["a", "b"]]`.

**Sources:**

- [GeeksforGeeks — Power set](https://www.geeksforgeeks.org/dsa/power-set/)
</details>

<details>
<summary>Determine if a binary representation of an integer is a palindrome.</summary>

### Determine if a binary representation of an integer is a palindrome.

**Definition:**

Compare bits from the two ends of the significant width, ignoring leading zeros. GeeksforGeeks times that scan at O(log x) time and O(1) space. Building the binary string and reversing it is also O(log x) time but O(log x) space.

**How it works:**

- The width is `floor(log2 x) + 1` for x > 0. Zero is a palindrome; the page returns true.
- Bit `l` and bit `r` must match while `l < r`. `9` is `1001`, which matches. `10` is `1010`, which does not.
- Reversing only the significant bits and comparing to `x` is the same test: `rev == x`.
- A fixed-width reverse that keeps leading zeros answers a different question.

**Examples:**

```text
def is_binary_palindrome(x):
    if x == 0:
        return True
    width = x.bit_length()
    left, right = 0, width - 1
    while left < right:
        if ((x >> left) & 1) != ((x >> right) & 1):
            return False
        left += 1
        right -= 1
    return True
```

`is_binary_palindrome(9)` is true. `is_binary_palindrome(10)` is false.

**Sources:**

- [GeeksforGeeks — Binary representation palindrome](https://www.geeksforgeeks.org/dsa/check-binary-representation-number-palindrome/)
</details>

<details>
<summary>Find the minimum and maximum of two integers without using comparisons.</summary>

### Find the minimum and maximum of two integers without using comparisons.

**Definition:**

An arithmetic right shift of the difference copies the sign bit into a mask, so the smaller value can be selected without a `<` in the source. GeeksforGeeks gives this form and times it at O(1) time and O(1) space, provided `x - y` itself does not overflow a signed word.

**How it works:**

- Let `d = x - y`. On a 32-bit arithmetic shift, `d >> 31` is all ones when `d` is negative and zero when `d` is not.
- Minimum is `y + (d & mask)`. When `x < y`, the mask keeps `d` and the sum is `x`. Otherwise the mask is 0 and the result is `y`.
- Maximum is `x - (d & mask)`.
- The page’s other form, `y ^ ((x ^ y) & -(x < y))`, still uses a comparison to build the mask. The shift form is the one that matches a ban on comparison operators.
- The subtraction precondition is part of the method. If `x - y` wraps, the sign bit is wrong.

**Examples:**

```text
def vmin(x, y):
    d = x - y
    return y + (d & (d >> 31))

def vmax(x, y):
    d = x - y
    return x - (d & (d >> 31))
```

`vmin(3, 5)` is 3. `vmax(3, 5)` is 5. This assumes a 32-bit arithmetic shift and a difference that fits.

**Sources:**

- [GeeksforGeeks — Min or max without branching](https://www.geeksforgeeks.org/dsa/compute-the-minimum-or-maximum-max-of-two-integers-without-branching/)
</details>

<details>
<summary>Check if two integers have opposite signs using bit manipulation.</summary>

### Check if two integers have opposite signs using bit manipulation.

**Definition:**

XOR sets the sign bit when the two sign bits differ. GeeksforGeeks states `(a ^ b) < 0` as the test, in O(1) time and O(1) space.

**How it works:**

- In two’s complement the top bit is the sign. XOR of opposite signs leaves that bit set, so the result is negative.
- Same signs leave the sign bit clear.
- Examples from the page: `(-5) ^ 10` is negative, so they differ; `7 ^ 15` is not.
- A product test also works if the multiply is done in a wider type so it cannot overflow. XOR does not multiply.
- Zero is non-negative, so it does not count as the opposite of a positive number.

**Examples:**

```text
def opposite_signs(a, b):
    return (a ^ b) < 0
```

`opposite_signs(-5, 10)` is true. `opposite_signs(7, 15)` is false.

**Sources:**

- [GeeksforGeeks — Opposite signs](https://www.geeksforgeeks.org/dsa/detect-if-two-integers-have-opposite-signs/)
</details>
