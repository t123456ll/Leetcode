### 204  Count Primes

1/200

Count the number of prime numbers less than a non-negative number, **n**.

##### Exmples:

```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```



##### Background:

Prime number: a whole number greater than 1 whose only factors are 1 and itself. 

**Sieve of Eratosthenes**: It does so by iteratively marking as composite (i.e., not prime) the multiples of each prime, starting with the first prime number, 2.  

埃氏筛：是一种高效寻找质数的方法

```java
class Solution {
    public int countPrimes(int n) {
        boolean[] notPrime = new boolean[n];
        int count = 0;
        for(int i = 2; i < n; i++){
            if(!notPrime[i]){
                count++;
                for(int j=i; j < n; j = j + i){ //cannot make it j=i*i, could not figure why
                    notPrime[j] = true;
                }
            } 
        }
        return count;        
    }
}
```

