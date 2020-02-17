# SEIR传染病模型netlogo仿真程序
![](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3583499134,1389607878&fm=11&gp=0.jpg "face")

2019年年底，一场突如其来的传染病席卷中国湖北武汉，新型冠状病毒感染的肺炎疫情悄然蔓延，从武汉波及全国，牵动人心。受到B站某UP主仿真程序的启发，
想通过计算机仿真实现一个更加真实的病毒传播仿真模型，下面是我的仿真模型的基本介绍和模型演示。

## 1、SEIR模型
![](https://pic3.zhimg.com/80/v2-1c8ce33920e7ac4c1e691af3df8b5dd6_hd.png "")

*图片来源：知乎-信号处理工程师的日常*

SEIR模型在[SIR](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology#The_SIR_model)模型的基础之上新增加了一个潜伏者（Exposed），这和新冠病毒实际情况更加符合，易感染人群在一开始会经历潜伏期，一段时间之后才出现症状，李兰娟院士认为，潜伏期患者也有可能具有传染性，潜伏者按照一定概率转化为感染者，因此，SEIR微分方程为：

![](https://www.zhihu.com/equation?tex=dS%2Fdt%3D-r%5Cbeta+IS%2FN%5C%5C+dE%2Fdt%3Dr%5Cbeta+IS%2FN-%5Calpha+E%5C%5C+dI%2Fdt%3D%5Calpha+E-%5Cgamma+I%5C%5C+dR%2Fdt%3D%5Cgamma+I "方程1")

*SEIR微分方程*

上式中α对应的感染概率，β对应的是潜伏者转换为感染者的概率，γ对应的治愈概率，r是接触的人数。该方程实质上反映的是易感染者S(t)、潜伏者E(t)、感染者I(t)、康复者R(t)单位时间变化数量的随时间t的变化情况，它们之间会相互影响，下面为迭代形式：

![](https://www.zhihu.com/equation?tex=S_n%3DS_%7Bn-1%7D-r%5Cbeta+I_%7Bn-1%7DS_%7Bn-1%7D%2FN%5C%5C+E_n%3DE_%7Bn-1%7D%2Br%5Cbeta+I_%7Bn-1%7DS_%7Bn-1%7D%2FN-%5Calpha+E_%7Bn-1%7D%5C%5C+I_n%3DI_%7Bn-1%7D+%2B%5Calpha+E_%7Bn-1%7D-%5Cgamma+I_%7Bn-1%7D%5C%5C+R_n%3DR_%7Bn-1%7D%2B%5Cgamma+I_%7Bn-1%7D "方程2")

*SEIR迭代方程*

## 2、模型假设
* 新型冠状病毒主要的传播途径还是呼吸道飞沫传播和接触传播，气溶胶和粪—口等传播途径尚待进一步明确。通过流行病学调查显示，病例多可以追踪到与确诊的病例有过近距离密切接触的情况，因此，模型假设只有接触传播一种传播途径。
* 为了简化模型，模型假设感染者不会死亡，只会传染其他易感染者或者被隔离并治愈。
* 模型假设康复者体内含有抗体，不会再感染病毒。
* 模型假设潜伏者没有被收治，感染者会被收治，被收治的感染者会一直占用医院隔离区域
* 模型假设被隔离的感染者无法再感染其他人
* 模型假设患者只能通过医院收治再被治愈

## 3、模型参数
|  参数   | 说明  | 类型 |
|  ----  | ----  | ---- |
| population  | 总人数  | 整数 |
| hospital-patient-segregation-area  | 医院隔离区域 | 整数 |
| human-flow-range | 人们的活动最大范围 | 整数 |
| infection-rate | 感染概率 | 浮点数 |
| transform-rate | 潜伏者转换为感染者的概率 | 浮点数 |
| recovery-rate | 康复概率 | 浮点数 |
| initial-infectious-num | 初始感染人数 | 整数 |
| latent-time | 潜伏期 | 整数 |
| receive-cure-response-time | 开始收治病人的延迟时间 | 整数 |
| receive-rate | 收治效率 | 浮点数 |

## 4、模型运行
我以“为什么要进行自我隔离”问题为例来演示模型的运行，也可以通过[web版模型](https://github.com/dpoqb/netlogo_SEIR_2019/blob/master/2019-SEIR.html)在浏览器中自行尝试其他实验。
假设有以下三种情形：a、人们都满世界乱跑、b、人们每天只见街坊邻居、c、人们宅在家只和家人呆在一起

这三种情形通过human-flow-range参数来模拟，human-flow-range是相对每个人家庭位置的活动范围，abc分别对应human-flow-range的值为50、10、2。下面为各主体人数变化情况

![](https://github.com/dpoqb/netlogo_SEIR_2019/blob/master/lab1-img1.png)
*human-flow-range = 50*

![](https://github.com/dpoqb/netlogo_SEIR_2019/blob/master/lab1-img2.png)
*human-flow-range = 10*

![](https://github.com/dpoqb/netlogo_SEIR_2019/blob/master/lab1-img3.png)
*human-flow-range = 2*

通过对比发现，人们如果不进行隔离而去接触许多人的话，被感染的人数峰值会很大很多，
