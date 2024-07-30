# The possibility of rebuilding the original data based on n-gram

(still working)

The article aims to develop a model that calculates the probabilities of reconstructing the original data when using an n-gram to generate a vocabulary from a text repository, which cannot be published publicly due to copyright or privacy concerns.

## Proof

1. We have a text, T.

2. Text T contains a word sequence: $\\{w_1, w_2, w_3, w_4, ..., w_k\\}$, where *k* is the total number of words in the sequence.

3. An n-gram creates a subset of text T:

   $$\\{s_1, s_2,...,s_{k-n+1}\\} = \\{\\{w_1,..., w_n\\}, \\{w_2,..., w_{1+n}\\}, ..., \\{w_{k-n+1}, ..., w_k\\}\\}$$

   Thus, the probability of any subset of T, denoted as $s_{m+1}$, is given by:

   $$Pr(s_{m+1}|s_m) = \frac{Pr(s_m|s_{m+1}) Pr(s_{m+1})}{Pr(s_m)}$$

4. The probability of finding the correct sequence of T for $\\{s_1, s_2,...,s_{k-n+1}\\}$ is:

   $$Pr(s_1, s_2, ..., s_{k-n+1}) = Pr(s_1)Pr(s_2 | s_1)Pr(s_3 | s_2)...Pr(s_{k-n+1} | s_{k-n})$$

   $$= Pr(s_1)\left(\frac{Pr(s_2|s_1)Pr(s_2)}{Pr(s_1)}\right)\left(\frac{Pr(s_3|s_2)Pr(s_3)}{Pr(s_2)}\right)...\left(\frac{Pr(s_{k-n}|s_{k-n+1})Pr(s_{k-n+1})}{Pr(s_{k-n})}\right)$$

   $$= Pr(s_2|s_1)Pr(s_3|s_2) \ldots Pr(s_{k-n}|Pr_{k-n+1}) \ldots Pr(s_{k-n}|s_{k-n+1})Pr(s_{k-n+1})$$

5. The method by which we match $s_{m+1}$ from $s_m$ is to use the overlap subsequence between $s_{m+1}$ and $s_m$, which is $\{w_{m+1}, \ldots, w_{m+n-1}\}$. The length of this subsequence is $n-1$.

6. Based on (4) and (5), we can simplify $Pr(s_{m+1}|s_m)$ as the expectation of the possibility of subsequence $s_{n-1}$, denoted as $exp(s_{n-1})$.

7. Based on (4) and (6), we get :
  
   $$Pr(s_1, s_2, ..., s_{k-n+1}) = Pr(s_1) exp(s_{n-1})^{(k-n)}Pr(s_{k-m+1})$$
   
	The possibilities of the first and last sequence are quite similar, therefore we can reach this conclusion:

   $$Pr(s_1, s_2, ..., s_{k-n+1}) = Pr(s_1)^2 exp(s_{n-1})^{(k-n)}$$

~~10. Sometimes, we assume that if $x\\%$ of the data was rebuilt, information is leaked. Thus, we can modify our model as follows:~~
    
~~Pr(s_1, s_2, ..., s_{k \cdot x\\% - n + 1}) = Pr(s_1)^2 exp(s_{n-1})^{(k \cdot x\\% - n)}~~
