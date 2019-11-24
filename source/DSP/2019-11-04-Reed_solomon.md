# Reed Solomon Coding

## Introduction
This article describes the Reed Solomon channel coding.

## Reed Solomon 

Basics on linear codes: proakis book.
Linear codes have simple math relationship between each codewords, which is designed to detect and correct errors based on redundant parity check information.


Finite Fields:
 
Finite fields, some basic characteristics:
GF(p^1)is extension field of GF(p), GF(p) is ground field of GF(p^m), p is the characteristic of GF(p).
i.e. GF(2^4 = 16) has ground field GF(2). A codeword in GF(16) would be 0001, while there is only 0 or 1 in GF(2). We will use GF(2) and its extension field for demonstration.
The addition/subtraction in GF(2) is XOR, multiplication/division are as usual.

Here we define a polynomial of degree m over GF(2):
g(x) = g_0 + g_1x + g_2x^2 + ... + g_mx^m
where g_i are elements in GF(2) (0 or 1), and g_m = 1 (when g_m = 1 in some GF(p), the polynomial is called monic).
From now on, we will use the term codeword and polynomial representation of codeword interchangeably. The codeword are information bits that we use in the system, and its polynomial representation is used for calculation (it defines the math relationship between each codewords). 
The polynomial addition is in GF(2), multiplication is as usual. 

When the polynomial can not be decomposed to product of two nonzero degree polynomials, we call it irreducible.
i.e. X^2 + 1 = X^2 + (1 xor 1)X + 1 = (X + 1)^2 is not irreducible.
When a monic polynomial is irreducible, we call it a prime polynomial.

If we have a prime polynomial g(x) with degree n, we can use it to construct a GF(2^n) extension field. 
i.e. for GF(2^3), assume we have any two codeword A, B (maybe 001, 101), we can get another codeword by C' = A'xB' mod g(x), where A', B', C' are polynomial representation of codewords.
You may find that the reminder of division by g(x) would have degree not larger than 3, and the combination of g_0, g_1, g_2 will form the GF(2^3).

Some of the prime g(x) will led to interesting results: The every result codeword of extension field can be written as powers of x. i.e. A' = x^1 mod g(x), B' = x^3 mod g(x), C' = x^4 mod g(x). This is not saying C' = 1000 (this codeword does not exist in GF(2^3)), but that C' can be represented as power of x in GF(2^3) field.

What is exactly the x then, is it only a symbol for codeword representation or it is evaluable?
It depends, when finding the roots of polynomial over GF(2), x will be evaluate to codeword of GF(2^m).
When representing codeword in GF(2^m), it is a symbol.
For clarity, we will use x as a evaluable variable, and alpha as a symbol representing codeword in GF(2^m).

Let us continue. 
For beta in GF(2^m), the smallest value of i such that beta^i = 1 is called the order of beta.
When the order of beta = 2^m-1, we call beta primitive element.
The prime g(alpha) that will result alpha being primitive element, is called primitive polynomial of GF(2^m). All roots of a primitive polynomial of degree m are primitive elements of GF(2^m).

**The primitive polynomial is handy that the result alpha has power that can cover any codeword in GF(2^m)**. Therefore when doing multiplication, we can ship the mod part by first looking up the corresponding power for codeword and sum them to find result power and then translate back to codeword (similar with converting  production to logarithmic addition, AxB = exp(log(A) + log(B))).
**We always assume that Galois fields are constructed using primitive polynomials.**

Here we present a example of GF(2^4^) with primitive polynomial X^4 + X + 1:  
Elements of GF(16)

| Power | Polynomial | Vector |
| :---: | ---: | :---: |
| - 		| 0  						| 0000 |
| a^0=a^15	| 1  						| 0001 |
| a^1  	 	| a   						| 0010 |
| a^2  	 	| a^2  						| 0100 |
| a^3  	 	| a^3  						| 1000 |
| a^4  	 	| a + 1 					| 0011 |
| a^s  	 	| a^2 + a					| 0110 |
| a^6  	 	| a^3 + a^2     			| 1100 |
| a^7  	 	| a^3 + a+ 1     		    | 1011 |
| a^8  	 	| a^2 + 1       			| 0101 |
| a^9  	 	| a^3 + a 					| 1010 |
| a^10 	 	| a^2 + a + 1 				| 0111 |
| a^11 	 	| a^3 + a^2 + a 			| 1110 |
| a^12	 	| a^3 + a^2 + a + 1  		| 1111 |
| a^l3 	 	| a^3 + a^2 + 1 			| 1101 |
| a^14   	| a^3 + 1 					| 1001 |

It shows that all the elements in GF(2^4) can be represented as power of alpha.

Minimal polynomial of GF(2^m) field element alpha' is the lowest-degree monic polynomial g(x) over the ground field that has the element alpha' as its root.

The minimal polynomial is for elements in extension fields, but the polynomial itself has coefficients defined in ground field.
The minimal polynomial can be found by first find minimal t that make alpha^(2^t-1) = 1. Then the minimal polynomial:
theta(alpha) = \product\from{i=0}\to{i=t-1} (x + alpha^(2^i)) over GF(2)
The element alpha' in GF(2^m) will only equal to 1 **over GF(2)** when there is 1 in alpha'.
Note that alpha^(2^i), i = 1, 2,..., t-1 share the same minimal polynomial, and we call them conjugates of alpha.

All nonzero elements are the root of x^(2^m - 1) - 1 = 0.
**primitive polynomials is only used for definition in GF. The minimal polynomial is used for generator polynomial in cyclic codes**.


linear block code

A linear block code C a k-dimensional subspace of an n-dimensional space which is usually called an (n, k) code. For binary codes, a linear block code is a collection of 2^k binary sequences of length n (k info bits, n - k parity redundant bits), such that for any two codewords c1, c2 \in C we have c1 + c2 \in C.

In a linear block code, the mapping from the set of M = 2^k information sequences of length k to the corresponding 2^k codewords of length n can be represented by a k x n matrix G called the generator matrix as
c_m = u_m G
where U_m is a binary vector of length k denoting the information sequence and c_m is the corresponding codeword. The additions are in GF(2). **Here the generator matrix can be arbitrary, and different generator matrix correspond to different linear codes**.

If the generator matrix G has the following structure
G =[I_k | P]
where I_k is a k x k identity matrix and P is a k x (n - k) matrix, the resulting linear block code is called systematic. In systematic codes the first k components of the codeword are equal to the information sequence, and the following n - k components, called the parity check bits, provide the redundancy for protection against errors. It can be shown that any linear block code has a systematic equivalent.

The parity check matrix is given by
H = [P^t | I]
which obviously satisfies GH^t = 0.

The minimum distance of a code is the minimum of all possible distances between distinct codewords of the code, i.e., for repetition (2, 1) code, the distance is 2.

There are many linear codes used in practice, such as repetition, Hamming. The Hamming code is a (2^m - 1, 2^m - 1 - m) block code, its parity check matrix H consist of all possible binary vectors of length m excluding the all-zero vector.




cyclic linear codes

Cyclic codes are an important class of linear block codes. **Additional structure built in the cyclic code family (generator polynomial) makes their algebraic decoding at reduced computational complexity possible**.
Cyclic codes are a subset of the class of linear block codes that satisfy the cyclic shift property: **a cyclic shift of a codeword is also a codeword**. As a consequence of the cyclic property, the codes possess a considerable amount of structure which can be exploited in the encoding and decoding operations.
There are two important classes of cyclic codes, the BCH and Reed-Solomon codes.

if c(X) represents a codeword in a cyclic code, then X^i c(X) mod (X^n + 1) is also a codeword of the cyclic code.
**We can generate a cyclic code by using a generator polynomial g(X) of degree n-k. The generator polynomial of an (n, k) cyclic code is a factor of x^n + 1. Here the 'generator polynomial' is first proposed, and the generator matrix can be derived by generator polynomial**.
Codewords possessing the cyclic property can be generated by multiplying the 2^k^ message polynomials with a unique polynomial g(X).
It is clear from above that an (n, k) cyclic code can exist **only if we can find a polynomial g(X) of degree n - k that divides x^n + 1**. Therefore the problem of designing cyclic codes is equivalent to the problem of finding factors of x^n + 1. We have studied this problem for the case where n = 2^m - 1 for some positive integer m, and we have seen that for this case the factors of x^n + 1 are the minimal polynomials corresponding to the conjugacy classes of nonzero elements of GF(2^m).

The Systematic generator matrix can be derived from the generator polynomial g(X): lth row of G is x^(n-l) + R_l(X), where R_l(x) = x^(n-l) mod g(x). 
Systematic cyclic code may be generated by using shift registers with feedback.

On decoder side: 
y(X) = c(X) + e(X)
since c(X) is a codeword, it is a multiple of g(X), the generator polynomial of the code; i.e., c(X) = u(X)g(X) for some u(X), a polynomial of degree at most k - 1.
y(X) = u(X)g(X) + e(X)
From this relation we conclude
y(X) mod g(X) = e(X) mod g(X)
Let us define s(X) = y(X) mod g(X) to denote the remainder of dividing y(X) by g(X) and call s(X) the syndrome polynomial, which is a polynomial of degree at most n-k-1.

The division of y(X) by the generator polynomial g(X) may be carried out by means of a shift register which performs division.
Given the syndrome from the (n - k )-stage shift register, a table lookup may be performed to identify the most probable error vector. This will give the ML results. 
The table lookup decoding method using the syndrome is practical only when n- k is small. This method is impractical for many interesting and powerful codes, since it needs 2^(n-k) table entries.


The cyclic structure of the code can be used to simplify finding the error polynomial.
Meggit decoder:
To obtain the syndrome corresponding to y_1, we need to multiply s(X) by X and then divide by g(X).
By precomputing error of a table E of the syndromes of the error patterns of x^(n-1) + e, where e represent a error polynomial with degree n-2 and at most t - 1 nonzero coefficients.
Then by shifting the syndrome, we can always find a y_i that has a error at MSB position x^(n-1), then we can lookup the table and correct the error.
This method needs a table size of 1 + (n-1) + (n-1)^2 + ... + (n-1)^(t-1) ~ (n-1)^(t-1).

Example of cyclic code: Cyclic Hamming Codes with n = 2^m - 1 and n - k = m.
Maximum-Length Shift Register Codes: (n,k) =(2^m -l,m), which is dual of hamming code.


BCH codes

**BCH codes have rich algebraic structure (more structured generator polynomial) that makes their decoding possible by using efficient algebraic decoding algorithms**. In addition, BCH codes exist for a wide range of design parameters (rates and block lengths) and are well tabulated. It also turns out that BCH codes are among the best-known codes for low to moderate block lengths.
In this section we treat only a special class of binary BCH codes called primitive binary BCH codes. These codes have a block length of n = 2^m - 1 for some integer m >= 3, and they can be designed to have a guaranteed error detection capability of at least t errors for any t < 2^(m-1). 

To design a t-error correcting (primitive) BCH code, we choose alpha, a primitive element of GF(2^m). Then g(X), the generator polynomial of the BCH code, is defined as the lowest-degree polynomial g(X) over GF(2) such that alpha, alpha^2, alpha^3 , ... , and alpha^2t are its roots.
Using the definition of the minimal polynomial, we can find the generator polynomial. Since alpha, alpha^2, alpha^4,... are conjugates, we only need the minimal polynomial of alpha, alpha^3, alpha^5,..., alpha^(2t-1).

For a BCH codeword c(X), we know that g(X) is a divisor of c(X). Therefore, **all alpha^i for i = 1,...,2t are roots of c(X)**. Using this information, we can design decoding algorithm that is more efficient than other cyclic codes.

Decoding BCH Codes

Since BCH codes are cyclic codes, any decoding algorithm for cyclic codes can be applied to BCH codes. For instance, BCH codes can be decoded using a Meggit decoder. However, the **additional structure** in BCH codes makes it possible to use more efficient decoding algorithms, particularly when using codes with long block lengths.

Let us denote the value of y(X) at alpha^i^ by S_i;, i.e., the syndromes defined by  
S_i = y(alpha^i) = c(alpha^i) + e(alpha^i) = e(alpha^i), i <= 2t
Note the calculation is over GF(2^m).

Now let us assume there have been v errors in transmission of c, where **v <= t**. Let us denote the location of these errors by j_1, j_2, ..., j_v,  where without loss of generality, we may assume 0 <= j_1 < j_2 < ... j_v <= n - 1. Therefore  
e(x) = x^j_v + x^j_(v-1) + ... + x^j_1
Then we have  
S_i = e(alpha^i) = (x^j_v)^i + (x^j_(v-1))^i + ... + (x^j_1)^i

These are a set of 2t equations with v unknowns.
Define beta_i = alpha^j_i, we can compose a polynomial:  

sigma(x) = (1 + beta_1 X)(1 + beta_2 X)...(1 + beta_v X)
		 = sigma_vX^v + ... + sigma_1X + 1
		 
The root of sigma(x) should be beta_i^-1. If we solve the polynomial and find its roots, we will get the error locations (j_i).
By examining sigma_v and S_i, we have the following 2t equations:  
S_1 + sigma_1 = 0  
S_2 + sigma_1S_1 + 2sigma_2 = 0
S_3 + sigma_1S_2 + sigma_2S_1 + 3sigma_3 = 0
...  
S_v + sigma_1S_(v-1) + ... + sigma_(v-1)S_1 + vsigma_v = 0  
S_(v+l) + sigma_1S_v + ··· + sigma_(v-1)S_2 + sigma_vS_1 = 0  
...

We need to obtain the lowest-degree polynomial (assuming error probability is small) sigma(X) whose coefficients satisfy this set of equations. One way of solving this problem is using  [Peterson–Gorenstein–Zierler algorithm](https://en.wikipedia.org/wiki/BCH_code#Peterson%E2%80%93Gorenstein%E2%80%93Zierler_algorithm).
Another efficient way doing so is to use the Berlekamp-Massey Decoding Algorithm.

To implement the Berlekamp-Massey algorithm, we begin by finding a polynomial of lowest degree sigma^l(X) that satisfies the first equation in 2t equations. In the second step we test to see if sigma^l(X) satisfies the second equation. If it satisfies the second equation, we set sigma^2(X) = sigma^l(X). Otherwise, we introduce a correction term to obtain sigma^2(X), making it to be the lowest degree polynomial that satisfies the first two equations. This process is continued until we obtain a polynomial of minimum degree that satisfies all equations.

**This is a iterative process**. If we assume that sigma^u(x) (with degree l_u) is the polynomial that satisfy the first u equation, then for the (u+1)th equation, we have:  
d_u = S_(u+1) + sigma_1^(u)S_u + ... + sigma_lu^(u)S_(u+1-lu)  
where is the (u+1)sigma_(u+1) term in the equation? we will cover that later.
If d_u = 0, we know the sigma^u(x) satisfy (u+1)th equation. Otherwise we have to set the sigma^(u+1)(x) as:  
sigma^(u+1)(x) = sigma^(u)(x) + d_u d_rou^-1 sigma^rou(x) x^(u-rou) 
where rou < u is selected such that d_p != 0 and among all such rou's the value of rou -l_rou is maximum.
The equation above may seem overwhelming at first. But it is not hard to understand when you have a example:  
The binary received sequence at the output of a (15, 7) BCH code with t=2 is  
y = (0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1)
Then the received polynomial is y(x) = x^3 + 1 (MSB first). The 2t = 4 syndromes in GF(2^) are  
S_1 = alpha^3 + 1 = alpha^14  
S_2 = (alpha^3)^2 + 1 = alpha^13  
S_2 = (alpha^3)^3 + 1 = alpha^7  
S_2 = (alpha^3)^4 + 1 = alpha^11  

| u 	| sigma^u(x) 						| d_u 		| I_u 	| u - I_u 	| equation 	|
| :---: | :--- 								| :---: 	| :---: | :---: 	| :---		|
| -1	| 1									| 1 		| 0 	| -1 		| 			|
| 0 	| 1									| alpha^14 	| 0 	| 0 		| S_1 + sigma_1 = d_u = alpha^14, sigma_1 = 0			|<-correction needed
| 1 	| 1 + alpha^14x						| 0	        | 1     | 0         | S_2 + sigma_1S_1 + 2sigma_2 = d_u = 0, sigma_2 = 0			|
| 2 	| 1 + alpha^14x 					| alpha^2	| 1     | 1         | S_3 + sigma_1S_2 + sigma_2S_1 + 3sigma_3 = d_u = alpha^2, sigma_3 = 0			|<-correction needed
| 3 	| 1 + alpha^14x + alpha^3x^2 		| 0	        | 2     | 1         | 1			|
| 4 	| 1 + alpha^14x + alpha^3x^2 		|	        | 2     | 2         | 1			|
	
A brief description of the calculations in the table:  
when u = -1, initialize states sigma^-1(x) = 1, d_u = 1, however no correction can be done.  
when u = 0, sigma^0(x) = 1, d_u = S_1 = alpha^14, we can do correction and rou = -1 (the only term with d_u !=0 and rou < 0). Note that sigma_1 = 0 in sigma^0(x). 
when u = 1, sigma^1(x) = sigma^0(x) + alpha^14 1^-1 sigma^-1(x) x^(0 - -1) = 1 + alpha^14x.  d_u = 0, sigma_2 = 0.  
when u = 2, sigma^2(x) = sigma^1(x).  
...  
We can see that **for any u, the term sigma_(u+1) = 0, therefore there is no (u+1)sigma_(u+1) term in d_u's equation**.  
The decompose the correction term d_u d_rou^-1 sigma^rou(x) x^(u-rou):  
For sigma^rou(x), we have  
d_rou = S_(rou+1) + sigma_1^rou S_rou + ... + sigma_i^rou S_(rou + 1 - i).  
Then for sigma^u(x), we have  
d_u = S_(u+1) + sigma_1^uS_u + ... + sigma_j^u S_(u + 1 - j).  
It is clear that we can cancel d_u by 
d_u + d_rou d_u d_rou^-1 = 0  
Then by align the RHS of above equations by S_l we have  
S_(u+1) + sigma_1^uS_u + ... + (sigma_(u + 1 - rou)^u + d_u d_rou^-1sigma_1^rou) S_rou + ... + (sigma_j^u + d_u d_rou^-1sigma_(j - u + rou)^rou) S_(u + 1 - j) + ... + d_u d_rou^-1 sigma_i^rou S_(rou + 1 - i) = 0  
Note that we have u + 1 - j > rou + 1 - i. In polynomial presentation, we have  
sigma^(u+1)(x) = sigma^(u)(x) + d_u d_rou^-1 sigma^rou(x) x^(u-rou)  
Also we have sigma^(u+1)(x) satisfying the first u equations, since the sigma_i for i < u-rou is not changed in sigma^(u+1)(x), while the first i equations only constrain the sigma_j, j < i. Using mathematical induction, we can prove that the correction term would not affect the first u equations.
By choosing value of rou having maximum rou -l_rou, we can build the lowest degree correction term.

After we found the sigma(x) satisfying the 2t equations, we can solve it to find error locations.
In the above example we have sigma(X) = 1 + alpha^14x + alpha^3x^2, and since the degree of this polynomial is 2, this corresponds to a correctable error pattern (2 <= t). We can find the roots of alpha(X) by inspection, i.e., by substituting the elements of GF(2^4). This will give the two roots of 1 and alpha^12 . Since the roots are the reciprocals of the error location numbers, we conclude that the error location numbers are {a^0, a^-12 = a^(-12+15) = alpha^3} . From this the errors are at locations j_1 = 0 and j_2 = 3. We have c(X) = y(X) + e(X) = 0, i.e., the detected codeword, is the all-zero codeword.
	
	
	
	
	
	
Reed Solomon

A good article about Reed Solomon coding: [Reed Solomon](https://www.embeddedrelated.com/showarticle/1182.php)



More detail about Reed Solomon decoding: [Original decoding](https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction#Peterson%E2%80%93Gorenstein%E2%80%93Zierler_decoder)

