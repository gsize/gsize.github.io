---
layout: post
title:  关于Geant4中的物理过程 
description: 描述Geant4中各个物理过程的层级结构
category: blog
---

   最近在修改[HPGe_simulation][1]程序中如何引入直接从粒子发生器(`G4VUserPrimaryGeneratorAction`)中输入衰变粒子即可得到该粒子的gamma能谱。
只要在PhysicsList中加`G4RadioactiveDecayPhysics`类,即可实现对衰变核素的gamma跃迁进行模拟。顺便了解Geant4中物理过程(PhysicsList)的组成。


------------------------------------
## 粒子物理基础知识

------------------------------------

先复习一下粒子物理知识，世间万物由以下粒子组成（除了最后一个），类似于面向对象编程中的对象——类，它们之间相互作用类似为类的操作。
粒子种类如下：
*  gluon/quarks/di-quarks（胶子、夸克类）
*  leptons（轻子 如：电子、muon子、tao子）
*  mesons（介子）
*  baryons（重子  如：质子、中子）
*  ions（离子  原子序数大于1的核子）
*  others（G4Geantino Geant4虚构出来的粒子）

其中，重子和介子统称为强子。

目前的物理模型中，粒子作用概括为四种作用，分别为：
*  引力  （所有有质量的粒子）
*  弱相互作用 （夸克、轻子、电弱规范玻色子）
*  电磁作用  （所有带电粒子）
*  强相互作用 （所有带色荷粒子，如：夸克、胶子）


------------------------------------
## 关于Geant4中的物理过程

------------------------------------

用户在写Geant4应用程序时，要么自己实现`PhysicsList`，要么直接使用集成好的物理过程。
Geant4实现的物理作用集中在强作用与电磁作用两方面。


------------------------------------
### 手动实现PhysicsList

在Geant4中，用户的`PhysicsList`类一般继承自`G4VUserPhysicsList`或`G4VModularPhysicsList`这两个类。
PhysicsList包括粒子种类，对应的物理作用类型，以及相关的能量截断值(cut)设置。

早期在搭建自己的物理过程时会选用前者，现在写PhysicsList使用后者。因为`G4VModularPhysicsList`继承自`G4VUserPhysicsList`，
并提供了更多灵活的操作函数，可以模块化选择所需要的物理过程。
下面是`G4VModularPhysicsList`相关的操作函数：

``` {.cpp}
    // Register Physics Constructor 
	void RegisterPhysics(G4VPhysicsConstructor* );
	 
	const G4VPhysicsConstructor* GetPhysics(G4int index) const;
	const G4VPhysicsConstructor* GetPhysics(const G4String& name) const;
	const G4VPhysicsConstructor* GetPhysicsWithType(G4int physics_type) const;

	// Replace Physics Constructor 
	//  The existing physics constructor with same physics_type as one of
	//  the given physics constructor is replaced
	//  (existing physics will be deleted)
	//  If a corresponding physics constructor is NOT found, 
	//  the given physics constructor is just added         
    void ReplacePhysics(G4VPhysicsConstructor* );

	// Remove Physics Constructor from the list
    void RemovePhysics(G4VPhysicsConstructor* );
    void RemovePhysics(G4int type);
    void RemovePhysics(const G4String& name);
```															       
通过调用`RegisterPhysics()`函数简便加入所需的物理过程，`ReplacePhysics()`加入新的或替换已存在同类型的物理过程，
`RemovePhysics()`移除物理过程。利用`G4VModularPhysicsList`操作的各类物理过程实现代码都在Geant4源代码physics_list/constructors目录之中，
其中包含了decay、electromagnetic、gammat_lepto_nuclear、hadron_elastic、hadron_inelastic、ions、limiters、stopping。PhysicsList类中具体实现如下：

``` {.cpp}
// EM physics
	RegisterPhysics(new G4EmLivermorePhysics());

// Decay
	RegisterPhysics(new G4DecayPhysics());

// Radioactive decay
	RegisterPhysics(new G4RadioactiveDecayPhysics());

```

同时，`G4VModularPhysicsList`类实现自动调用`void ConstructParticle()`和`void ConstructProcess()`函数，
无需像`G4VUserPhysicsList`重写(override)这两个函数。


------------------------------------
### 使用G4PhysListFactory调用集成的物理过程

目前，Geant4集成了很多可直接调用的`PhysicsList`类，这些物理过程可以在源代码[G4PhysListFactory][]，
也可以在[Geant4网站][2]中看到它们的[适用范围][3]。
这些集成的物理过程如下所列：

*   FTFP_BERT
*	FTFP_BERT_TRV
*	FTFP_BERT_HP
*	FTFP_INCLXX
*   FTFP_INCLXX_HP
*	FTF_BIC
*	LBE
*	QBBC
*   QGSP_BERT
*	QGSP_BERT_HP
*	QGSP_BIC
*	QGSP_BIC_HP
*	QGSP_BIC_AllHP
*   QGSP_FTFP_BERT
*	QGSP_INCLXX
*	QGSP_INCLXX_HP
*	QGS_BIC
*   Shielding
*	ShieldingLEND
*	ShieldingM
*	NuBeam

涉及到电磁作用的物理过程可索引[网站][6]查看详细信息，目前有以下7个

*	\_EMV —— `G4EmStandardPhysics_option1()`
*	\_EMX —— `G4EmStandardPhysics_option2()`
*	\_EMY —— `G4EmStandardPhysics_option3()`
*   \_EMZ —— `G4EmStandardPhysics_option4()`
*	\_LIV —— `G4EmLivermorePhysics()`
*	\_PEN —— `G4EmPenelopePhysics()`
*	\__GS —— `G4EmStandardPhysicsGS()`


这些集成的物理都可以直接声明使用，但为了程序对调用不同类型物理过程的灵活性，一般使用
[G4PhysListFactory][]实现模拟前物理过程的快速切换，详细可见例子[Hadr00][]，
下面是放在`main()`函数中关于[G4PhysListFactory][]的用法。

``` {.cpp}
	G4PhysListFactory factory;
	G4VModularPhysicsList* phys = 0;
	G4String physName = "";
	
	// Physics List name defined via 3nd argument
	if (argc>=3) { physName = argv[2]; }
	
	// Physics List is defined via environment variable PHYSLIST
	if("" == physName) {
	char* path = getenv("PHYSLIST");
	if (path) { physName = G4String(path); }
	}
	
	// if name is not known to the factory use FTFP_BERT
	if("" == physName || !factory.IsReferencePhysList(physName)) {
	physName = "FTFP_BERT"; 
	}
	
	// reference PhysicsList via its name
	phys = factory.GetReferencePhysList(physName);
	
	runManager->SetUserInitialization(phys);
```

通过上面的代码可发现，在运行程序前先设置好环境变量`PHYSLIST`，即可实现不同物理过程的调用。
例如，`export PhysList=FTFP_BERT_LIV`定义好环境变量后，应用程序将自动调用`FTFP_BERT()`和`G4EmLivermorePhysics()`这两个物理过程，实现强作用和电磁作用的调用。

也能采用`PhysicsListMessager`来管理物理过程的选择，方便在mac文件中调用。
在PhysicsList类中添加一个`AddPackage(const G4String& name)`，实现代码如下：

```{.cpp}
	#include "G4PhysListFactory.hh"
	void PhysicsList::AddPackage(const G4String& name)
	{
		G4PhysListFactory factory;
		G4VModularPhysicsList* phys =factory.GetReferencePhysList(name);

		for (G4int i = 0; ; ++i) {
			G4VPhysicsConstructor* elem =
			const_cast<G4VPhysicsConstructor*> (phys->GetPhysics(i));
			if (elem == NULL) break;
				G4cout << "RegisterPhysics: " << elem->GetPhysicsName() << G4endl;
				//RegisterPhysics(elem);
				ReplacePhysics(elem);
		}
	}
```

其中，不用`RegisterPhysics(elem)`，改用`ReplacePhysics(elem)`，因为前者在相同类型的物理过程已经存在时无法被替换。

------------------------------------

其实，Geant4软件有相当完善的资料提供给开发人员及用户，这些资料在[Geant4用户支持][7]页面之中。
里面包含了：

* [Geant4历年组织的培训资料][8] 
* [代码索引][9] 
* [常见问题及解答][10] 
* [Geant4论坛][11] 
* [Geant4相关文档][12] 
* [各类学习例子][13] 
* [电磁相互作用物理过程][14] 
* [强相互作用物理过程][15] 
* [Geant4应用程序的计算性能优化提示][16] 



[Hadr00]:  http://www-geant4.kek.jp/lxr/source/examples/extended/hadronic/Hadr00/Hadr00.cc "Hadr00"

[G4PhysListFactory]:  http://www-geant4.kek.jp/lxr/source/physics_lists/lists/src/G4PhysListFactory.cc    "G4PhysListFactory"

[Gsize]:    http://gsize.github.io  "Gsize"
[1]:  https://github.com/gsize/HPGe_simulation "HPGe_simulationulation"
[2]:  http://geant4.web.cern.ch/geant4/support/proc_mod_catalog/physics_lists/referencePL.shtml "物理过程索引"
[3]:  http://geant4.web.cern.ch/geant4/support/proc_mod_catalog/physics_lists/useCases.shtml "物理过程索引"
[4]:  http://www-public.slac.stanford.edu/geant4/  "斯坦福大学Geant4"
[5]:  http://geant4.in2p3.fr/  "法国in2p3实验室Geant4"
[6]:  http://geant4.cern.ch/collaboration/working_groups/electromagnetic/physlist.shtml#PL "电磁相互作用"
[7]:  http://geant4.cern.ch/support/index.shtml  "Geant4用户支持"

[8]:  http://geant4.cern.ch/support/training.shtml  "历年培训资料"
[9]: http://www-geant4.kek.jp/Reference/   "代码索引"
[10]:  http://geant4.cern.ch/support/faq.shtml  "常见问题及解答"
[11]:  http://hypernews.slac.stanford.edu/HyperNews/geant4/cindex  "论坛"
[12]:  http://geant4.cern.ch/support/userdocuments.shtml  "Geant4各类文档"
[13]:  http://cern.ch/geant4/UserDocumentation/Doxygen/examples_doc/html/  "Geant4各类例子"
[14]:  http://geant4.cern.ch/collaboration/working_groups/electromagnetic/physlist.shtml  "电磁相互作用"
[15]: http://geant4.cern.ch/support/proc_mod_catalog/physics_lists/physicsLists.shtml "强相互作用"
[16]:  https://twiki.cern.ch/twiki/bin/view/Geant4/Geant4PerformanceTips  "Geant4性能优化"

