TextMatrix是基于标准C++开发的一个跨平台(Windows,Linux)的用于文本分类或挖掘的开源项目。最早开始于2006年底，是我在学习C++的过程中逐渐开发的。
可以同时处理中英文文本(目前支持的中文文本编码格式是GB2312,GBK)，具有中文分词，英文Stemming,停用词滤除，抽取基于字或词的Ngram特征，生成基于SuffixTree的KeySubStringGroup特征，生成特征词表，按各类权重公式计算特征权重，降维，性能评测等功能，其输出文件兼容SVMlight,libsvm,fastann,cluto等软件所需的格式。功能的试验过程中参考了很多的相关文献和软件。

该软件项目提供了有以静态链接库(.lib或.a文件)形式的使用接口，并编写了NaiveBayesian,K-NearestNeighbor机器学习方法用来演示如何使用 TextMatrix基础接口的使用方法。提供主要文本分类功能的TextMatrix主程序以命令行的形式提供，便于在实验中批量和定制运行。

为了展示在大批量的实验中如何使用TextMatrix从头到位完成从文本处理到最终性能评测运行的全过程，我也同时公开了一个Linux下的基于 shell的文本分类试验运行包，其主要试验是完成对文本的基于后缀树的KeySubStringGroup特征额抽取和文本分类。其中展示了TextMatrix的部分功能(未使用中文分词，降维等功能)。具体使用方法可以参见TextMatrix帮助手册，文本试验包中的语料库则是我公开的 16000篇宾馆评论情感分类语料库v1.0。

项目从概念上来分，包括文本预处理(分词，stemming,ngram生成，词表生成),特征权重向量生成，降维，性能评测几大部分。其中英文单词的Stemming功能使用了Snowball【1】,中文分词参考了【2】,在此基础上改进和支持了GBK编码,词表来自 北大的共享资源，部分权重公式参考了【3】,以及chi^2降维方法公式参考了【4】,性能评测的代码参考了Lingpipe项目【5】的相应Java代码,人工神经网络则是基于Fastann的基于c的库【6】。基于后缀树的KeySubStringGroup抽取代码基于Dell Zhang在KDD06所发表的论文【7】所附带的仅支持英文的演示性代码【8】基础上修改的，在与他的邮件来往中，我获得帮助与指导，将其改进为支持中文处理，并解决了数据量大时提示out of memory的问题。

用于存储多个文本向量可以看做是一个特征与文档的矩阵，不存在的特征被认为是0。为了更有效地利用空间，采用稀疏矩阵存储。实际的数据结构是用STL中的Map实现的稀疏矩阵类，头文件和主文件分别是SparseMatrix.h和SparseMatrix.cpp,实现了基本的对稀疏矩阵的访问，然后TextMatrix继承了SparseMatrix类，并进一步实现了更多与文本处理有关的相关功能。这里的矩阵只是用于存储与处理多个文本向量，与线性代数矩阵运算中矩阵概念不同，项目叫做TextMatrix,也是来源于此。

考虑到在Windows和Linux平台下都有处理文本的需要，实现时尽量采用了标准C++和STL编码，用同一份代码可以在Microsoft Visual Studio 2005，DevCpp4.9, gcc 同时编译。

由于项目实现的时间比较早，仍然有很多要改进的地方，比方说类的声明和定义接口不够清晰。还有许多功能未实现。K-NearestNeighbor 方法在文本上万的时候太慢，需要用kdd树之类的提速；文本编码应该支持UTF8；基于后缀树抽取子串特征的KeySubString在支持中文时采用的是一种经过简化的基于单字节的方案，有时间改成以汉字为处理单位；添加更多的机器学习方法；更多的分词，降维或特征选择的方法。

相关软件，文本分类试验包，数据集均可以从我的[主页](http://nlp.csai.tsinghua.edu.cn/~lj/)下载

参考文献：
```
【1】 Stemming Algorithms: Snowball. http://snowball.tartarus.org/. 
【2】 陈小何,现代汉语自动分析, 北京语言文学大学出版社,2000. 
【3】 Salton G, Buckley C. Term Weighting Approaches in Automatic Text Retrieval. Technical report, Ithaca, NY, USA, 1987. 
【4】 Yang Y, Pedersen J O. A Comparative Study on Feature Selection in Text Categorization. 
Proceedings of ICML ’97: Proceedings of the Fourteenth International Conference on Machine Learning, San Francisco, CA, USA: Morgan Kaufmann Publishers Inc., 1997. 412–420. 
【5】 LingPipe Project. http://www.alias-i.com/lingpipe/. 
【6】 Multilayer Artificial Neural Network Library: FastAnn. http://fann.sourceforge.net/. 
【7】 Zhang D, LeeWS. Extracting key-substring-group features for text classification. Proceedings of In Proceedings of the 12th ACM SIGKDD international conference on Knowledge discovery and data mining (KDD.06. ACM Press, 2006. 474–483.
【8】 The C program for Key-Substring-Group feature extraction. http://www.dcs.bbk.ac.uk/~Dell/publications/dellzhang kdd2006 supplement.html. 
【9】 Li J, Sun M. Experimental Study on Sentiment Classification of Chinese Review using Machine Learning Techniques. Proceedings of Procedings of IEEE International Conference on Natural Language Processing and Knowledge Engineering, 2007.
```