# ListLayerFrame
一个分层的列表框架，类似于MiniFilter的分层过滤框架

// Author: Zoo
//	QQ：276793422
//
//	首先，感谢Linux 内核源码吧，有几个宏是从里面拿出来的
//	然后，感谢Windows 的部分开源的代码，LIST_HEAD 就是从里面拿出来的
//
//	用途：
//		当前模块的存在，是为了实现一个类似MiniFilter的分层过滤接口
//
//	接口：
//		接口部分需要提供3个主要的函数指针列表
//
//		这两个函数列表的主要功能是为了实现跨平台，一旦平台确定，这两个函数列表实现之后可以万年不改
//		为了跨平台，支持多环境，这里做了太多的事情
//			LIST_LAYER_MEMORY_FUNCTION		主模块的初始化函数支持
//			LIST_INFO_MEMORY_FUNCTION		分层信息的初始化函数支持
//
//		这个函数列表的唯一用途就是
//			LIST_RULE_MEMORY_FUNCTION		每一层的规则支持函数
//
//	优点：
//		分层框架和规则列表分开了，
//		分层框架需要外部提供两套函数来做内存相关、锁相关的支持
//		规则列表部分需要外部自己实现规则列表的搭建、规则增、删、查
//		分层框架和规则列表分开了，所以替换规则列表比较方便，比如存某些数据用hash表好，或者存其他数据用红黑树好，
//			可以给外部很大很灵活的空间
//
//	缺点：
//		由于我考虑到需要适配多平台，所以我把内存相关、锁相关、平台相关可能的接口都导出了
//			这导致出现一个问题，就是外部需要做的工作很多，初始化起来很可能有点麻烦
//			但是如果确定选择某一平台的话，只需要初始化一次就好了，
//		目前接口部分仍然不是很精练，仍然有优化空间
//		目前内部函数部分仍然有些不易读，内部函数接口部分仍然需要修改一下
//		内部某些锁的使用方法及位置有可能不是很合理，
//		这个代码，实际上代码部分，完成度不算低，主要的问题还是UT比较少
//
//
//
//		我已经尽量把代码写得好看了，但是有些地方还是不太好看
//
