# 文本相似度算法

## BM25Similarity

> BM25 Similarity. Introduced in Stephen E. Robertson, Steve Walker, Susan Jones, Micheline Hancock-Beaulieu, and Mike Gatford. Okapi at TREC-3. In Proceedings of the Third Text REtrieval Conference (TREC 1994). Gaithersburg, USA, November 1994.

[wikipedia](https://en.wikipedia.org/wiki/Okapi_BM25)
[Okapi at TREC-3](http://trec.nist.gov/pubs/trec3/papers/city.ps.gz)

### 算法函数

> BM25 is a bag-of-words retrieval function that ranks a set of documents based on the query terms appearing in each document, regardless of the inter-relationship between the query terms within a document (e.g., their relative proximity). It is not a single function, but actually a whole family of scoring functions, with slightly different components and parameters. One of the most prominent instantiations of the function is as follows.

Given a query $Q$, containing keywords $q_{1},...,q_{n}$, the BM25 score of a document $D$ is:

![score_BM25.svg](./docs/score_BM25.svg)

$${\text{score}}(D,Q)=\sum _{i=1}^{n}{\text{IDF}}(q_{i})\cdot {\frac {f(q_{i},D)\cdot (k_{1}+1)}{f(q_{i},D)+k_{1}\cdot \left(1-b+b\cdot {\frac {|D|}{\text{avgdl}}}\right)}}$$

$f(q_i, D)$ is $q_{i}$'s term frequency in the document $D$, $|D|$ is the length of the document $D$ in words, and `avgdl` is the average document length in the text collection from which documents are drawn.

$f(q_i, D)$     词元$q_i$在文档D中的频率(term frequency),即`tf(t in d)`,`t means q`,
$|D|$           文档$D$的长度,
`avgdl`         集合中文档的平均长度.

`k1`和b是变量,一般,$k_1 \in [1.2,2.0]$ and $b = 0.75$

```JAVA
    /**
     * BM25 with the supplied parameter values.
     * @param k1 Controls non-linear term frequency normalization (saturation). 控制非线性词元频率归一化(饱和).
     * @param b Controls to what degree document length normalizes tf values. 控制文档长度归一化tf值的程度.
     */
    public BM25Similarity(float k1, float b) {
        this.k1 = k1;
        this.b  = b;
    }

    /** BM25 with these default values:
     * <ul>
     *   <li>{@code k1 = 1.2},
     *   <li>{@code b = 0.75}.</li>
     * </ul>
     */
    public BM25Similarity() {
        this.k1 = 1.2f;
        this.b  = 0.75f;
    }
```

$\text{IDF}(q_i)$ is the IDF (inverse document frequency) weight of the query term $q_{i}$. It is usually computed as:

$$\text{IDF}(q_i) = \log \frac{N - n(q_i) + 0.5}{n(q_i) + 0.5}$$

```JAVA
    /** Implemented as <code>log(1 + (numDocs - docFreq + 0.5)/(docFreq + 0.5))</code>. */
    protected float idf(long docFreq, long numDocs) {
        return (float) Math.log(1 + (numDocs - docFreq + 0.5D)/(docFreq + 0.5D));
    }
```

### IDF信息理论解释

