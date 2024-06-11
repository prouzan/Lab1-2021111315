声明：这里的问题描述是形式化描述，略去类似于读取文本字符并将其转化为单词列表这样的繁琐过程。
## 2.1 功能1：文本转为有向图
### 输入：
列表：$words：\forall \ word \ \in words,\ word \ is \ a \ string$  
$ \ and \ \forall \alpha \in word ,\  \alpha \ \in [a-zA-Z]$

### 输出：
二维列表有向图：$graph = [weight_{ij}]_{n \times n},$  
索引映射(一个字符串型列表)：$nodes: N \to (string)^*$，其中$N$是nodes列表的下标（整数）集合，  $(string)^*$是(输入)符号串的集合；  
索引下标映射（一个整形列表）：$map: N_1  \to  N_2$，其中N1是$words$下标的集合，N2是$graph$中行下标$n$的集合；

### 约束关系：
$\forall word1,word2 \in words;$  
$if \ word2.iindex == word1.iindex + 1:$
$graph[word1.index_{nodes}][word2.index_{nodes}] += 1$  
其中$iindex$表示word在words中的下标，$index_{nodes}$表示word在nodes中的下标，也就是在graph中的索引。  
自然语言描述为：如果两个word在words中相邻，那么在前的word与其后继的word会有一条有向边，并且如果有多次相邻，那么边的权值就是相邻的次数。

## 2.2 功能2：查询桥接词
### 输入：
二维列表有向图：$graph = [weight_{ij}]_{n \times n}$;  
输入单词集合:$words$;  
索引映射(一个字符串型列表)：$nodes: N \to (string)^*$，其中$N$是nodes列表的下标（整数）集合，  $(string)^*$是(输入)符号串的集合；  
索引下标映射（一个整形列表）：$map: N_1  \to  N_2$，其中N1是$words$下标的集合，N2是$graph$中行下标$n$的集合；

### 输出：
单词(桥接词)的集合$B: \forall b \in B, \ b \in words$

### 约束关系：
$\forall word1,word2 \in words;$  
$if \exist word3 \in words \ st. \ word3.iindex == word1.iindex + 1 $  $and \ word3.iindex == word2.iindex - 1:$  
$B.append(word3)$  
其中$iindex$表示word在words中的下标，$index_{nodes}$表示word在nodes中的下标，也就是在graph中的索引。  
自然语言描述为：如果两个word之间有另一个word同时与他们两直接相连，那么中间这个word就是其他两个word的桥接词。word间的桥接词可能有多个，所以采用列表存储。

## 2.3 功能3：生成新文本
### 输入：
二维列表有向图：$graph = [weight_{ij}]_{n \times n}$;  
输入单词集合:$words$;  
索引映射(一个字符串型列表)：$nodes: N \to (string)^*$，其中$N$是nodes列表的下标（整数）集合，  $(string)^*$是(输入)符号串的集合；  
索引下标映射（一个整形列表）：$map: N_1  \to  N_2$，其中N1是$words$下标的集合，N2是$graph$中行下标$n$的集合；  
新单词列表：$words_N$

### 输出：
输出单词列表：$word_{out}$

### 约束关系：
$\forall word1,word2 \in words_N;$  
$if \exist word3 \in words \ st. \ word3 \in Bridge(word1, word2)$  
$Insert(word1, word2, word3)$  
其中$Bridge(word1, word2)$表示在$word1$和$word2$之间的桥接词的集合，$Insert(word1, word2, word3)$表示将$word3$插入到$word1$和$word2$之间。（仅插入$Bridge(word1, word2)$中的一个）

## 2.4 功能4：最短路径
### 输入：
二维列表有向图：$graph = [weight_{ij}]_{n \times n}$;  
文本输入单词集合:$words$;  
输入单词：$word1,word2$  
索引映射(一个字符串型列表)：$nodes: N \to (string)^*$，其中$N$是nodes列表的下标（整数）集合，  $(string)^*$是(输入)符号串的集合；  
索引下标映射（一个整形列表）：$map: N_1  \to  N_2$，其中N1是$words$下标的集合，N2是$graph$中行下标$n$的集合； 

### 输出：
输出路径列表：$Path$

### 约束关系：
对于给定的有向图 $G = (V, E)$，其中 $V$ 表示节点集合，$E$ 表示边的集合，以及节点集合的索引映射 $nodes$ 和边的索引映射 $map$，
以及给定的起始单词 $word1$ 和目标单词 $word2$，通过Dijkstra算法找到最短路径：

$Path = Dijkstra(G, nodes, word1, word2)$

其中，$Dijkstra$ 是用于求解最短路径的算法，返回从 $word1$ 到 $word2$ 的最短路径 $Path$。

## 2.5 功能5：随机游走
### 输入：
二维列表有向图：$graph = [weight_{ij}]_{n \times n}$;  
文本输入单词集合:$words$;  
索引映射(一个字符串型列表)：$nodes: N \to (string)^*$，其中$N$是nodes列表的下标（整数）集合，  $(string)^*$是(输入)符号串的集合；  
索引下标映射（一个整形列表）：$map: N_1  \to  N_2$，其中N1是$words$下标的集合，N2是$graph$中行下标$n$的集合； 

### 输出：
输出路径列表：$S$

### 约束关系：
设有向图 $G = (V, E)$ 表示整个图，其中 $V$ 表示节点集合，$E$ 表示边的集合，以及节点集合的索引映射 $nodes$ 和边的索引映射 $map$。
随机游走从图中随机选择一个节点作为起点 $v_{\text{start}} \in V$，然后沿出边进行随机遍历，记录经过的所有节点和边，直到出现第一条重复的边为止，或者进入的某个节点不存在出边为止。
定义随机游走的节点序列为 $S = (v_{\text{start}}, v_1, v_2, \ldots, v_k)$，其中 $v_i \in V$ 表示第 $i$ 步所经过的节点。
定义随机游走的边序列为 $E = ((v_{\text{start}}, v_1), (v_1, v_2), \ldots, (v_{k-1}, v_k))$，其中 $(v_i, v_{i+1}) \in E$ 表示第 $i$ 步到第 $i+1$ 步所经过的边。

随机游走的约束关系可以用以下数学形式描述：

1. 选择起点：$v_{\text{start}} \in V$ 是一个随机选择的节点。
2. 遍历过程：从起点 $v_{\text{start}}$ 出发，进行随机游走，直到出现第一条重复的边或者某个节点不存在出边为止，即存在 $i \in \{1, 2, \ldots, k-1\}$，使得 $(v_i, v_{i+1}) \notin E$ 或者存在 $i, j \in \{1, 2, \ldots, k\}$ 且 $i < j$，使得 $v_i = v_j$。
3. 记录结果：将随机游走的节点序列 $S$ 输出
