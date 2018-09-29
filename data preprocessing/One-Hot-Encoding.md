https://www.cnblogs.com/king-lps/p/7846414.html

处理离散型特征和连续型特征并存的情况，如何做归一化？

1. 离散型特征的处理方法: 

a) Binarize categorical/discrete features: For all categorical features, represent them as multiple boolean features. For example, instead of having one feature called marriage_status, have 3 boolean features - married_status_single, married_status_married, married_status_divorced and appropriately set these features to 1 or -1. As you can see, for every categorical feature, you are adding k binary feature where k is the number of values that the categorical feature takes.

2. 为什么使用one-hot编码来处理离散型特征？

　1) Why do we binarize categorical features? 

　　We binarize the categorical input so that they can be thought of as a vecotr from the Euclidean space (we call this as embedding the vector in the Euclidean space). 

　　使用one-hot 编码,将离散特征的取值扩展到欧式空间,离散特征的某个取值就对应欧式空间的某个点.

　2) Why do we embed the feature vectors in the Euclidean-space? 

　　Because many algorithms for classification/regression/clustering etc. required computing distance between features or similarities between features. And many definitions of distances and similarities are defined over features in Euclidean space. So, we would like our features to lie in Euclidean space as well. 

　　将离散特征通过one-hot编码映射到欧式空间,是因为,在回归,分类,聚类等机器学习算法中,特征之间距离的计算或相似度的计算是非常重要的,而我们常用的距离或相似度的计算都是在欧式空间的相似度计算,计算余弦相似性,基于的就是欧式空间.

　3) Why does embedding the feature vector in Euclidean space require us to binarize categorical features? 

　　Let us take an example of a dataset with just one feature (say job_type as per your example) and let us say it takes three values 1,2,3. 
　　Now, let us take three feature vectors x1=(1), x2=(2), x3=(3). What is the euclidean distance between x1 and x2, x2 and x3 & x1 and x3? d(x1,x2)=1, d(x2,x3)=1, d(x1,x3)=2. This shows that distance between job type 1 and job type 2 is smaller the job type 1 and job type 3. Does this make sense? can we even rationally define a proper distance between different job types? In many cases of categorical features, we can properly define a proper distance between different values that the categorical feature takes. In such cases, isn't it fair to assume that all categorical features are equally far away from each other? 
　　Now, let us see what happens when we binary the same feature vectors. Then, x1=(1,0,0), x2=(0,1,0), x3=(0,0,1).Now, what are the distance between then? They are sqrt(2). So, essentially, when we binarize the input, we implicitly state that all values of the categorical features are equally away from each other.

　4) About the original question? 

　　Note that our reason for why binarize the categorical features is independent of the number of the values tha categorical features take, so yes, even if the categorical feautre takes 1000 values, we still would prefer to do binarization.

　5) Are there cases when we can avoid doing binarization? 

　　Yes. As we figured out earlier, the reason we binarize is because we want some meaningful distance relationship between the different values. As long as there is some meaningful distance relationship, we can avoid binarizing the categorical feature. For example, if you are building a classifier to classify a webpage as import entity page (a page important to particular entity) or not and let us say that you have the rank of the webpage in the search result for that entity as a feature, then 1] note that the rank feature is categorical, 2] rank1 and rank 2 are clearly closer to each other than rank1 and rank3, so the rank feature defines a meaningful distance relationship and so, in this case, we don't have to binarize the categorical rank feature. 
　　More generally, if you can cluster the categorical values into disjoint subset such that the subsets have meaningful distance relationship amongst them, then you don't have binarize fully, instead you can split then only over these clusters. For example, if there is a categorcial feature with 1000 values, but you can split thest 1000 values into 2 groups of 400 and 600 (say) and within each group, the values have meaningful distance relationship, then instead of fully binarizing, you can just add 2 features, one for each cluster and that should be fine.
　　It depends on you ML algorithms, some methods requires almost no efforts to normalize features or handle both continuous and discrete features, like tree based methods: C4.5, Cart, random Forrest, bagging or boosting. But most of parametric model(generalized linear models, neural network, SVM, etc) or methods using distance metrics(KNN, kernels, etc) will require careful work to achieve good results. Standard approaches including binary all features, 0 mean unit variance all continuous features, etc.

　6) 独热编码的优缺点 

　　优点: 独热编码解决了分类器不好处理属性数据的问题,在一定程度上也起到了扩充特征的作用.它的值只有0和1,不同的类型存储在垂直的空间. 

　　缺点: 当类型的数量很多时,特征空间会变得非常大.在这种情况下,一般可以用PCA来减少维度.而且one-hot encoding + PCA这种组合在实际中也非常有用

　7) Tree Model不太需要one-hot编码 

　　对于决策树来说,one-hot的本质是增加树的深度 
  
　　tree-model 是在动态的过程中生成类似 One-Hot + Feature Crossing的机制
  
　　一个特征或者多个特征最终转成一个叶子节点作为编码, one-hot可以理解成三个独立事件.
  
　　决策树是没有特征大小的概念的,只有特征处于它分布的哪一部分的概念.
