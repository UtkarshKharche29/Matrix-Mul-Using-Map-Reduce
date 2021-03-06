# Matrix-Multiplication-using-MapReduce (Hadoop)

## Purpose
The purpose of this program is using naive algorithm and advanced algorithm to implement matrix multiplication.

## Language
python 3

## Dataset
* Input:
  * matrix A: m * n,   index(i,j)
  * matrix B: n * p,  index(j,k)
  * **format**: matrix, row, col, value (see data/test_data.txt)
* Output:
  * result of A*B
  * **format**:(row, col), val

## Naive Algorithm
* For each A, repeat p pairs. For each B, repeat m pairs.
* Map:
```
  k = 0 to p-1:
    Matrix A -> KEY: (row, k)  VALUE: (A, col, value)
  k = 0 to m-1:
    Matrix B -> KEY: (k, col)  VALUE: (B, col, value)
```
* Reduce:
```
  KEY: (i, k) VALUE:[(A, 0, val), ...]
  res = 0
  k = 0 to n-1:
    res += A[i][j]*B[j][k]
```
## Smarter Algorithm
* The maps label A as **a row blocks** and B as **b col blocks**
* Map:
```
  k = 0 to p/b:
    Matrix A -> KEY: (row/a, k)  VALUE: (A, row, col, value)
  k = 0 to m/a:
    Matrix B -> KEY: (k, col/b)  VALUE: (B, row, col, value)
```
* Reduce:
```
  KEY: block num (a_block, b_block)
  VALUE: [(row, col, value),...,(...)]//numbers in the block

  // implement matrix multiplication of the blocks locally
  res = 0
  for i = row of ele in this a block:
    for k = col of ele in this b block:
      for j = 0 to n-1:
        res += A[i][j] * B[j][k]
```
## Test
* Test naive algorithm locally
```
  . test_naive.sh
```
* Test smarter algorithm locally
```
  . test_smarter.sh
```
* The output should be:
```
  (0, 0), 90.000000
  (0, 1), 100.000000
  (0, 2), 110.000000
  (1, 0), 240.000000
  (1, 1), 275.000000
  (1, 2), 310.000000
```



