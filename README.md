# rebuttals
Dear all,

1, "Updated Table 1.pdf" contains the updated results and some other classical results on AES-MMO/MP and Groestl.


2, There is a detailed anwser for replacing Grover search with certainty in Alg. 4 by Grover search without certainty as follows:

Q:
Second, quantum search is used as if it transforms an exact exhaustive search cost of 2^n to pi/4*2^{n/2}. For example, in Algorithm 4, step 4 "Run Grover search with certainty...", and the complexity analysis claims a cost of pi/4*2^{11/2}. This completely neglects the fact that, if you use [BHMT02] and if the number of solutions is unknown, we can be arbitrarily precise with a polynomial overhead. In these attacks, there is some variance in the number of solutions, as for example the number of solutions for the differential equation of the Sbox is either 2 or 4. Also, if you really want to do costs estimates with high precision, the probability that the equation has a solution is not exactly 1/2.

Answer: Agree. We will fix this issue by replace it by Grover search without certainty.  We compute the success probability of U_F.

Given nonzero input-output differences for Sbox of AES, the probability that it has solutions is 127/255. 

We take the first non-full SSB as example, corresponding to G^{(0)}. Please look at Fig. 4 in the submission paper, given input-output differences of SSB, we guess A[0], then \Delta B[0] is known. Then, all the other differences are determined due to MC. Now there are 4 active Sboxes whose input-output differences have to match through DDT of Sboxes, the probability is about (127/255)^4.

1)	If there is no solution by traversing 2^{11} for G^{(0)}. That means for each guessing of A[0], we do not have a valid matching for the 4 active Sboxes. The probability is p=(1-(127/255)^4)^{2^{8}}= 8.7*10^{-8}. In this case, run Grover on U_G, it will definitely output an invalid starting point. Since there are 4 non-full SSB, we get an invalid starting point with Pr=1-(1-p)^4, which is almost 0.

2)	If there are solutions by traversing 2^{11} for G^{(0)}. The probability is 1-8.7*10^{-8}. After about 1/arcsin{2^{-11/2}} \approx 2^{11/2}\approx 45.25 Grover iterations, we get the superposition with good states with probability of about 1-2^{-11} due to the Grover algorithm. We just consider the worst case that there is only one solution.

Since there are 4 SSB, for a given input of U_F, we get a valid starting point with probability (1- 8.7*10^{-8})^4*(1-2^{-11})^4=0.998. Therefore, we get a collision pair with probability Pr_s = 0.998*2^{-83}. Then we run about \sqrt{1/Pr_s} Grover iterations on U_F to get a collision. The complexity is very close to the submission paper.

