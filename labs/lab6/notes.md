# Κωδικοποίηση JPEG

1. Split image into 8 by 8 pixel blocks
    - It is easier to work with 8 by 8 blocks
    ```python
    vector_blocks = []
    iHeight, iWidth = img.shape
    # Image partitioned into 8x8 blocks
    for startY in range(0, iHeight, 8):
    for startX in range(0, iWidth, 8):
    block = img[startY:startY + 8, startX:startX + 8]
    vector_blocks.append(block)
    ```

2. Apply DCT on each block
    - Discrete Cosine Transform
    ```python
    block_f = np.float32(block) # <- this float32 will be the bit values of the pixels
    block_dct = cv2.dct(block_f)
    ```
    - After DCT (each block has:)
        - Upper left block is DC component, the rest are AC components
        - Vertical frequencies show from left to right
        - Horizontal frequencies show from top to bottom

3. Quantization on each block
    - Removes information from the image
    - Quantization for all (u,v) components
    ```math
    I(u,v) = int[I(u,v)/Q(u,v) + 0.5],
    where Q(u,v) is the given quantization matrix
    ```
    ```python
    # Quantization of the DCT coefficients
    # Multiply Q table by a coefficient. The factor will determine the amount of compression
    q_table_factor = np.multiply(q_table, factor) 
    block_q = np.floor(np.divide(block_dct, q_table_factor) + 0.5)
    ```

4. Rearrange data using zig zag order
    - Rearrange the data in zig zag order into a 1D array
    - The point is that firs appear the greater compontents and then the lower components
    ```
    a0 b0 c0 d0 e0 f0 g0 h0
    a1 b1 c1 d1 e1 f1 g1 h1
    a2 b2 c2 d2 e2 f2 g2 h2
    a3 b3 c3 d3 e3 f3 g3 h3
    a4 b4 c4 d4 e4 f4 g4 h4
    a5 b5 c5 d5 e5 f5 g5 h5
    a6 b6 c6 d6 e6 f6 g6 h6
    a7 b7 c7 d7 e7 f7 g7 h7
    ```
    - Turns into
    ```
    a0 b0 a1 a2 b1 c0 d0 c1 b2 a3 a4 b3 c2 d1 e0 e1 ... 
    ```
    - Order
    ```
    1  2  6  7  15 16
    3  5  8  14 17
    4  9  13 18
    10 12 19
    11 20
    21 
    22 ...
    ```

5. a. Take DC component and apply DPCM (Differential Pulse Code Modulation)
    - Subtract the DC component of the previous block from the current block
    ```python
    e = []
    # leave first value of first vector as it is 
    e.append(vectors_zigzag[0][0])
    for k in range(1, len(vectors_zigzag)):
        e.append(vectors_zigzag[k][0] - vectors_zigzag[k - 1][0])
    ```

5. b. Take the rest AC components and apply RLC (Run Length Coding)
    - In order to mitigate the zeros in the AC components
    - Uses (skip, value) pairs
    - In order to show the end of the block we append a (0, 0) pair
    ```
    0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 2
    -> Will be turned into:
    (5, 1) (0, 1) (8, 2) (0, 0)
    ```

6. Huffman encoding
    - Want to minimize the bits used per pixel value
    - Use a binary tree
    0. Delete pixel values with zero frequency%
    1. Find the 2 values with least frequency%
    2. Combine them into a node with frequency% = the sum of their frequencies
    3. Name one size of the node as 0 and the other as 1 (each edge is 0 or 1)
    4. Repeat from 1 until we have a single root with frequency% = 1
    