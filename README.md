# Imbalance Data Problem

In machine learning, we often encounter unbalanced data. For example, in a bank's credit data, 97% of customers can pay their loans on time, while only 3% cannot. If we ignore 3% of customers who cannot pay on time, the accuracy of the model may still be high, but it may bring huge losses to the bank. Therefore, we need appropriate methods to balance the data.

Many research papers provide many techniques including over-sampling and under-sampling, to deal with data imbalance. This repository implements some of those techniques. 

- [Requirements](https://github.com/zhu-y/Imbalance-Data#requirements)
- [Over-sampling: SMOTE](https://github.com/zhu-y/Imbalance-Data#smote)
- [Over-sampling: ADASYN](https://github.com/zhu-y/Imbalance-Data#adasyn)
- [References](https://github.com/zhu-y/Imbalance-Data#references)

## Requirements
```
sklearn
numpy
```

## SMOTE
SMOTE is a synthetic minority over-sampling technique mentioned in N. V. Chawla, K. W. Bowyer, L. O. Hall and W. P. Kegelmeyer's paper [SMOTE: Synthetic Minority Over-sampling Technique][1]

```
Parameters
----------
sample:     2D (numpy)array
            minority class samples
N:          Integer
            amount of SMOTE N%
k:          Integer
            number of nearest neighbors k
            k <= number of minority class samples
Attributes
----------
newIndex    Integer
            keep a count of number of synthetic samples
            initialize as 0
synthetic   2D array
            array for synthetic samples
neighbors   K-Nearest Neighbors model

```

The corresponding code is in [smote.py][2]. 

### Example
```python
from smote import Smote
import numpy as np

X = np.array([[1, 0.7], [0.95, 0.76], [0.98, 0.85], [0.95, 0.78], [1.12, 0.81]])
s = Smote(sample=X, N=300, k=3)
s.over_sampling()
print(s.synthetic)
```
The output will be:
```
[[0.9688157377661356, 0.7470434369118096], [0.970373970826427, 0.7203406632716296], [0.955180350748186, 0.7209519703266685], [0.95, 0.76], [0.9603507618011522, 0.7093355880188698], [0.95, 0.76], [0.98, 0.85], [0.98, 0.85], [0.9767000397651937, 0.8023105914068543], [0.95, 0.78], [0.95, 0.78], [0.9536226582758756, 0.8380845147770741], [1.025027934535906, 0.8276733346832177], [1.0691988855686414, 0.8064896755773396], [1.0457470065562635, 0.7305641034293823]]

```

### Visualize a Simple Example
Suppose the blue triangles are majority class data, the green triangles are minority class data. 
The red dots are the synthetic samples we generated by SMOTE.

![](https://github.com/zhu-y/Imbalanced-data/blob/master/image/smote_example_1.png)


## ADASYN
ADASYN is mentioned in Haibo He, Yang Bai, Edwardo A. Garcia, and Shutao Li's paper [ADASYN: Adaptive Synthetic Sampling Approach for Imbalanced Learning][3]

```
Parameters
-----------
X           2D array
            feature space X
            
Y           array
            label, y is either -1 or 1
            
dth         float in (0,1]
            preset threshold
            maximum tolerated degree of class imbalance ratio
            
b           float in [0, 1]
            desired balance level after generation of the synthetic data
            
K           Integer
            number of nearest neighbors
            
Attributes
----------
ms          Integer
            the number of minority class examples
            
ml          Integer
            the number of majority class examples
            
d           float in n (0, 1]
            degree of class imbalance, d = ms/ml
            
minority    Integer label
            the class label which belong to minority
            
neighbors   K-Nearest Neighbors model

synthetic   2D array
            array for synthetic samples
```

The corresponding code is in [adasyn.py][4]. 

### Example
```python
from adasyn import Adasyn
X = [[1, 1], [1.3, 1.3], [0.7, 1.2], [1.1, 1.1], [0.95, 1.3], [1.4, 1.4],
     [1.2, 1.5], [1.2, 1.7], [1.2, 1.3], [0.88, 0.9], [0.98, 0.55],
     [0.92, 1.24], [0.8, 1.35], [1, 2], [1.5, 1.5], [0.8, 1.8]]
Y = [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, 1, 1, 1]
a = Adasyn(X, Y, dth=1, b=1, K=4)
a.sampling()
print(a.synthetic)
```
The output will be:
```
[[1.0, 2.0], [0.8733313075577545, 1.8733313075577545], [1.5, 1.5], [1.5, 1.5], [1.5, 1.5], [1.5, 1.5], [0.9621350795352689, 1.962135079535269], [0.8593405970230131, 1.8593405970230132]]
```

### Visualize a Simple Example
Suppose the blue triangles are majority class data, the green triangles are minority class data. 
The red dots are the synthetic samples we generated by SMOTE.
![](https://github.com/zhu-y/Imbalance-Data/blob/master/image/adasyn_example_1.png)

## References:
- Haibo He, Yang Bai, Edwardo A. Garcia, and Shutao Li. ADASYN: Adaptive Synthetic Sampling Approach for Imbalanced Learning. <http://sci2s.ugr.es/keel/pdf/algorithm/congreso/2008-He-ieee.pdf>
- N. V. Chawla, K. W. Bowyer, L. O. Hall and W. P. Kegelmeyer. SMOTE: Synthetic Minority Over-sampling Technique. <https://arxiv.org/pdf/1106.1813.pdf>


[1]: https://arxiv.org/pdf/1106.1813.pdf
[2]: https://github.com/zhu-y/Imbalance-Data/blob/master/smote.py
[3]: http://sci2s.ugr.es/keel/pdf/algorithm/congreso/2008-He-ieee.pdf
[4]: https://github.com/zhu-y/Imbalance-Data/blob/master/adasyn.py
