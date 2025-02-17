# Some-Algorithm
论文
* Youyi Zheng, Hongbo Fu, Oscar Kin-Chung Au, and Chiew-Lan Tai. 2011. <b>Bilateral Normal Filtering for Mesh Denoising</b>. IEEE Transactions on Visualization and Computer Graphics 17, 10 (October 2011), 1521-1530.


**Local Scheme**
如截图所示---**Bilateral Normal Filtering for Mesh Denoising**
![1](1.png)
现手动计算,便于C++实现
$
\mathbf{n}_i^\prime=K_i \sum_{j\ni\N(i)} \zeta_{ij} 
W_c(
$$ 
    \begin{Vmatrix}
    \mathbf{c}_i-\mathbf{c}_j 
    \end{Vmatrix}  
$$)
W_s(
$$ 
    \begin{Vmatrix}
    \mathbf{n}_i-\mathbf{n}_j 
    \end{Vmatrix}  
$$)
\mathbf{n}_j 
$
其中
$
K_i=\frac{1}{\sum_{j\ni\N(i)} \zeta_{ij} 
W_c(    
    \begin{Vmatrix}
    \mathbf{c}_i-\mathbf{c}_j 
    \end{Vmatrix}  
    )
W_s( 
    \begin{Vmatrix}
    \mathbf{n}_i-\mathbf{n}_j 
    \end{Vmatrix}  
)}
$
$
W_c(\begin{Vmatrix} \mathbf{c}_i-\mathbf{c}_j \end{Vmatrix})=
$$
exp(-
    \begin{Vmatrix}
    \mathbf{c}_i-\mathbf{c}_j 
    \end{Vmatrix}  
^2 / 2\sigma_c^2)
$$=
$$
exp(-\frac{ \begin{Vmatrix}
    \mathbf{c}_i-\mathbf{c}_j 
    \end{Vmatrix}^2}{2\sigma_c^2})
$$
$
$
W_s(\begin{Vmatrix} \mathbf{n}_i-\mathbf{n}_j \end{Vmatrix})=
$$
exp(-
    \begin{Vmatrix}
    \mathbf{n}_i-\mathbf{n}_j 
    \end{Vmatrix}  
^2 / 2\sigma_s^2)
$$=
$$
exp(-\frac{ \begin{Vmatrix}
    \mathbf{n}_i-\mathbf{n}_j 
    \end{Vmatrix}^2}{2\sigma_s^2})
$$
$
$\mathbf{n}_i^\prime$可化简为
$
\mathbf{n}_i^\prime=\frac{\sum_{j\ni\N(i)} \zeta_{ij} 
exp(-\frac{ \begin{Vmatrix}
    \mathbf{c}_i-\mathbf{c}_j 
    \end{Vmatrix}^2}{2\sigma_c^2})
exp(-\frac{ \begin{Vmatrix}
    \mathbf{n}_i-\mathbf{n}_j 
    \end{Vmatrix}^2}{2\sigma_s^2}) \mathbf{n}_j }
{\sum_{j\ni\N(i)} \zeta_{ij} 
exp(-\frac{ \begin{Vmatrix}
    \mathbf{c}_i-\mathbf{c}_j 
    \end{Vmatrix}^2}{2\sigma_c^2})
exp(-\frac{ \begin{Vmatrix}
    \mathbf{n}_i-\mathbf{n}_j 
    \end{Vmatrix}^2}{2\sigma_s^2})}
$
其过程类似于
$
\mathbf{n}_i^\prime=\frac{K_1\mathbf{n}_1 + K_2\mathbf{n}_2+ K_3\mathbf{n}_3 + ...}{K_1 + K_2 + K_3 + ...}
$
相关变量
$K_i$:向量归一化系数
$N(i)$:面$f_i$的邻近面集合,可选择基于顶点/面
$c_i$:面$f_i$的中心坐标
$c_j$:面$f_j$的中心坐标
$n_i$:面$f_i$的法向量
$n_j$:面$f_j$的法向量
$W_c$:基于中心坐标的高斯函数
$W_s$:基于法向量的高斯函数
$\sigma_c$:中心坐标的高斯函数里的标准差
$\sigma_s$:法向量的高斯函数里的标准差
通过迭代,paper中给出次数为5,即可求出法向量

**Global Scheme**
如截图所示
![2](2.png)
推导上述公式,计算最终结果
$
Result=\argmin_{\mathbf{n}_i^\prime}(1 - \lambda)E_s + \lambda E_d =               
\argmin_{\mathbf{n}_i^\prime}(1 - \lambda)\sum_i A_i 
$$ 
    \begin{Vmatrix}
    \mathbf{n}_i^\prime - K_i \sum_{j\ni\N(i)} w_{ij} \mathbf{n}_j^\prime 
    \end{Vmatrix}^2  
$$ + \lambda \sum_i A_i
$$
    \begin{Vmatrix}
    \mathbf{n}_i^\prime - \mathbf{n}_i
    \end{Vmatrix}^2 
$$
$

其中
$
E_s=\sum_i A_i 
$$ 
    \begin{Vmatrix}
    \mathbf{n}_i^\prime - K_i \sum_{j\ni\N(i)} w_{ij} \mathbf{n}_j^\prime 
    \end{Vmatrix}^2  
$$      
$

$
E_d= \lambda \sum_i A_i
$$
    \begin{Vmatrix}
    \mathbf{n}_i^\prime - \mathbf{n}_i
    \end{Vmatrix}^2 
$$
$

$\mathbf{n}_i^\prime$:需要求的法向量
$\mathbf{n}_i$:已知面的法向量

又知
$
\sum_{j\ni\N(i)} w_{ij} \mathbf{n}_j^\prime = \sum_{i\ni\N(j)} w_{ji} \mathbf{n}_i^\prime
$

$
\Rightarrow Result =                
\argmin_{\mathbf{n}_i^\prime}(1 - \lambda)\sum_i A_i 
$$ 
    \begin{Vmatrix}
    \mathbf{n}_i^\prime - K_i \sum_{i\ni\N(i)} w_{ji} \mathbf{n}_i^\prime 
    \end{Vmatrix}^2  
$$ + \lambda \sum_i A_i
$$
    \begin{Vmatrix}
    \mathbf{n}_i^\prime - \mathbf{n}_i
    \end{Vmatrix}^2 
$$
$

-------------------------------------------------------------------
基于带有惩罚项的最小二乘法的数学原理,去除$\sum$, 可以将Result组装成矩阵
$
\Rightarrow Result = argmin_{N^\prime}
$$
(1 - \lambda) D (N^\prime - W N^\prime)^2 + \lambda D (N^\prime - N)^2
$$
$
对$N^\prime$求偏导,并令其为0,则有
$
\Rightarrow 2 N^\prime(1 - \lambda) D (1 - 2W + W^2) + \lambda D (2 N^\prime - 2 N)=0
$
$
\Rightarrow [(1 - \lambda)(1 - W)^2 + \lambda] N^\prime = \lambda N
$
其中
$N^\prime$:要求的未知量,也就是法向量
$N$:已知面的法向量
$W$:由基于中心坐标和法向量的高斯函数相乘得到,并用Eigen的setFromTriplets组装成矩阵
利用$AX=B$即可求出X,从原理上可以计算出结果
相关变量已经由Local Scheme给出,这里不再给出





























