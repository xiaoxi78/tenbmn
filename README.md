# 【基于飞桨实现乒乓球时序动作定位大赛 ：B榜第8名方案】
> 

## 项目描述
>【基于飞桨实现乒乓球时序动作定位大赛 ：B榜第8名方案】
使用飞桨提供的GPU资源，paddlepaddle框架，框架结构优美，希望能长期使用。

参考代码：

https://aistudio.baidu.com/aistudio/projectdetail/3434130?channelType=0&channel=0

参考文章：

《BMN 视频动作定位模型》 https://github.com/PaddlePaddle/PaddleVideo/blob/develop/docs/zh-CN/model_zoo/localization/bmn.md

《论文解析丨基于BSN算法改进的动作时序检测BMN算法论文解析》 https://god.yanxishe.com/columnDetail/28315

《ActivityNet Challenge 2019 冠军模型BMN算法全解析》https://blog.csdn.net/weixin_45449540/article/details/106394079

赛题介绍
在众多大规模视频分析情景中，从冗长未经修剪的视频中定位并识别短时间内发生的人体动作成为一个备受关注的课题。当前针对人体动作检测的解决方案在大规模视频集上难以奏效，高效地处理大规模视频数据仍然是计算机视觉领域一个充满挑战的任务。其核心问题可以分为两部分，一是动作识别算法的复杂度仍旧较高，二是缺少能够产生更少视频提案数量的方法（更加关注短时动作本身的提案）。

这里所指的视频动作提案是指一些包含特定动作的候选视频片段。为了能够适应大规模视频分析任务，时序动作提案应该尽可能满足下面两个需求： （1）更高的处理效率，例如可以设计出使时序视频片段编码和打分更高效的机制； （2）更强的判别性能，例如可以准确定位动作发生的时间区间。

本次比赛旨在激发更多的开发者和研究人员关注并参与有关视频动作定位的研究，创建性能更出色的动作定位模型。

数据集介绍
jupyter

本次比赛的数据集包含了19-21赛季兵乓球国际比赛（世界杯、世锦赛、亚锦赛，奥运会）和国内比赛（全运会，乒超联赛）中标准单机位高清转播画面的特征信息，共包含912条视频特征文件，每个视频时长在0～6分钟不等，特征维度为2048，以pkl格式保存。我们对特征数据中面朝镜头的运动员的回合内挥拍动作进行了标注，单个动作时常在0～2秒不等，训练数据为729条标注视频，A测数据为91条视频，B测数据为92条视频，训练数据标签以json格式给出。

数据集预处理
本方案采用PaddleVideo中的BMN模型。BMN模型是百度自研，2019年ActivityNet夺冠方案，为视频动作定位问题中proposal的生成提供高效的解决方案，在PaddlePaddle上首次开源。此模型引入边界匹配(Boundary-Matching, BM)机制来评估proposal的置信度，按照proposal开始边界的位置及其长度将所有可能存在的proposal组合成一个二维的BM置信度图，图中每个点的数值代表其所对应的proposal的置信度分数。网络由三个模块组成，基础模块作为主干网络处理输入的特征序列，TEM模块预测每一个时序位置属于动作开始、动作结束的概率，PEM模块生成BM置信度图。

本赛题中的数据包含912条ppTSM抽取的视频特征，特征保存为pkl格式，文件名对应视频名称，读取pkl之后以(num_of_frames, 2048)向量形式代表单个视频特征。其中num_of_frames是不固定的，同时数量也比较大，所以pkl的文件并不能直接用于训练。同时由于乒乓球每个动作时间非常短，为了可以让模型更好的识别动作，所以这里将数据进行分割。

模型分享：
![image](https://img-blog.csdnimg.cn/5dd57a40b9c3418ba18c5a988b2fd668.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbWVudG9yemY=,size_20,color_FFFFFF,t_70,g_se,x_16)
![image](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9zS2lhMUZLRmlhZmdnZVhscThKWlhjRTRrbzBUWm05ZzgxZUhiSE5XN0VuUFNZZzFpYVJpYktQaFdkMTFNamdSOFJxR0tPSG5HRXc1NWYxSGliaWFWQTBYVGg2QS82NDA?x-oss-process=image/format,png)

## 项目结构
> 一目了然的项目结构能帮助更多人了解，目录树以及设计思想都很重要~
```
-|data
-|work
-README.MD
-main.ipynb
```
## 使用方式
> 
A：在AI Studio上[运行本项目](https://aistudio.baidu.com/aistudio/projectdetail/3517534?contributionType=1)  
B：直接FORK此项目，还有免费算力使用，非常方便！
