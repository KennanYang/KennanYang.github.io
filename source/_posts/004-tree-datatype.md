---
title: 数据结构总结 —— 树
date: 2022-03-12 08:59:29
categories: 数据结构
tags: 
- c
- 数据结构
---

# 引言

[《c语言学习网》数据结构总结——树](http://c.biancheng.net/view/3390.html)
本文旨在根据教程巩固树的数据结构知识和c语言指针的应用，记录一下自己学习过程中的代码，知识讲解参考教程。
## 一、树的基本概念和术语

树结构是一种非线性存储结构，存储的是具有“一对多”关系的数据元素的集合。如图 是使用树结构存储的集合 {A,B,C,D,E,F,G,H,I,J,K,L,M} 的示意图。对于数据 A 来说，和数据 B、C、D 有关系；对于数据 B 来说，和 E、F 有关系。这就是“一对多”的关系。
![图 1（A）树的示例](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0NDMwMTQ5My0wLnBuZw?x-oss-process=image/format,png)
<!--more-->

### 1.树的节点
结点：使用树结构存储的每一个数据元素都被称为“结点”。例如，图 1（A）中，数据元素 A 就是一个结点；

父结点（双亲结点）、子结点和兄弟结点：对于图 1（A）中的结点 A、B、C、D 来说，A 是 B、C、D 结点的父结点（也称为“双亲结点”），而 B、C、D 都是 A 结点的子结点（也称“孩子结点”）。对于 B、C、D 来说，它们都有相同的父结点，所以它们互为兄弟结点。

树根结点（简称“根结点”）：每一个非空树都有且只有一个被称为根的结点。图 1（A）中，结点 A 就是整棵树的根结点。

> 树根的判断依据为：如果一个结点没有父结点，那么这个结点就是整棵树的根结点。

叶子结点：如果结点没有任何子结点，那么此结点称为叶子结点（叶结点）。例如图 1（A）中，结点 K、L、F、G、M、I、J 都是这棵树的叶子结点。
### 2.子树和空树
子树：如图 1（A）中，整棵树的根结点为结点 A，而如果单看结点 B、E、F、K、L 组成的部分来说，也是棵树，而且节点 B 为这棵树的根结点。所以称 B、E、F、K、L 这几个结点组成的树为整棵树的子树；同样，结点 E、K、L 构成的也是一棵子树，根结点为 E。

> 注意：单个结点也是一棵树，只不过根结点就是它本身。图 1（A）中，结点 K、L、F 等都是树，且都是整棵树的子树。

知道了子树的概念后，树也可以这样定义：树是由根结点和若干棵子树构成的。

空树：如果集合本身为空，那么构成的树就被称为空树。空树中没有结点。

> 补充：在树结构中，对于具有同一个根结点的各个子树，相互之间不能有交集。例如，图 1（A）中，除了根结点
> A，其余元素又各自构成了三个子树，根结点分别为 B、C、D，这三个子树相互之间没有相同的结点。如果有，就破坏了树的结构，不能算做是一棵树。

### 3.结点的度和层次
对于一个结点，拥有的子树数（结点有多少分支）称为结点的度（Degree）。例如，图 1（A）中，根结点 A 下分出了 3 个子树，所以，结点 A 的度为 3。

> 一棵树的度是树内各结点的度的最大值。图 1（A）表示的树中，各个结点的度的最大值为 3，所以，整棵树的度的值是 3。

结点的层次：从一棵树的树根开始，树根所在层为第一层，根的孩子结点所在的层为第二层，依次类推。对于图 1（A）来说，A 结点在第一层，B、C、D 为第二层，E、F、G、H、I、J 在第三层，K、L、M 在第四层。

> 一棵树的深度（高度）是树中结点所在的最大的层次。图 1（A）树的深度为 4。

如果两个结点的父结点虽不相同，但是它们的父结点处在同一层次上，那么这两个结点互为堂兄弟。例如，图 1（A）中，结点 G 和 E、F、H、I、J 的父结点都在第二层，所以之间为堂兄弟的关系。

### 4.有序树和无序树
如果树中结点的子树从左到右看，谁在左边，谁在右边，是有规定的，这棵树称为有序树；反之称为无序树。

> 在有序树中，一个结点最左边的子树称为"第一个孩子"，最右边的称为"最后一个孩子"。

拿图 1（A）来说，如果是其本身是一棵有序树，则以结点 B 为根结点的子树为整棵树的第一个孩子，以结点 D 为根结点的子树为整棵树的最后一个孩子。

### 5.森林
由 m（m >= 0）个互不相交的树组成的集合被称为森林。图 1（A）中，分别以 B、C、D 为根结点的三棵子树就可以称为森林。

前面讲到，树可以理解为是由根结点和若干子树构成的，而这若干子树本身是一个森林，所以，树还可以理解为是由根结点和森林组成的。用一个式子表示为：

```
> Tree =（root,F）
```

其中，root 表示树的根结点，F 表示由 m（m >= 0）棵树组成的森林。
## 二、二叉树及其性质
简单地理解，满足以下两个条件的树就是二叉树：
1.本身是有序树；
2.树中包含的各个节点的度不能超过 2，即只能是 0、1 或者 2；
例如，图 1 a) 就是一棵二叉树，而图 1 b) 则不是。

![图 2 a)  二叉树](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0NTJMUjEtMC5naWY)
### 1.二叉树的性质
二叉树具有以下几个性质：
1.二叉树中，第 i 层最多有 2^i-1^ 个结点。
2.如果二叉树的深度为 K，那么此二叉树最多有 2^K^-1 个结点。
3.二叉树中，终端结点数（叶子结点数）为 n~0~，度为 2 的结点数为 n~2~，则 n~0~=n~2~+1。

> 性质 3 的计算方法为：对于一个二叉树来说，除了度为 0 的叶子结点和度为 2 的结点，剩下的就是度为 1 的结点（设为 n1），那么总结点
> n=n~0~+n~1~+n~2~。 同时，对于每一个结点来说都是由其父结点分支表示的，假设树中分枝数为 B，那么总结点数 n=B+1。而分枝数是可以通过
> n~1~ 和 n~2~ 表示的，即 B=n~1~+2*n~2~。所以，n 用另外一种方式表示为 n=n~1~+2*n~2~+1。 两种方式得到的 n
> 值组成一个方程组，就可以得出 n~0~=n~2~+1。

二叉树还可以继续分类，衍生出满二叉树和完全二叉树。
### 2.满二叉树
如果二叉树中除了叶子结点，每个结点的度都为 2，则此二叉树称为满二叉树。

满二叉树示意图

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0NTJIRzgtMS5naWY)
图 2 满二叉树示意图

如图 2 所示就是一棵满二叉树。

满二叉树除了满足普通二叉树的性质，还具有以下性质：
满二叉树中第 i 层的节点数为 2^n-1^ 个。
深度为 k 的满二叉树必有 2^k-1^ 个节点 ，叶子数为 2^k-1^。
满二叉树中不存在度为 1 的节点，每一个分支点中都两棵深度相同的子树，且叶子节点都在最底层。
具有 n 个节点的满二叉树的深度为 log~2~(n+1)。

### 3.完全二叉树
如果二叉树中除去最后一层节点为满二叉树，且最后一层的结点依次从左到右分布，则此二叉树被称为完全二叉树。

完全二叉树示意图

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0NTJNYjUtMi5naWY)
图 3 完全二叉树示意图

如图 3a) 所示是一棵完全二叉树，图 3b) 由于最后一层的节点没有按照从左向右分布，因此只能算作是普通的二叉树。

完全二叉树除了具有普通二叉树的性质，它自身也具有一些独特的性质，比如说，n 个结点的完全二叉树的深度为 ⌊log~2~n⌋+1。

> ⌊log~2~n⌋ 表示取小于 log~2~n 的最大整数。例如，⌊log~2~4⌋ = 2，而 ⌊log~2~5⌋ 结果也是 2。

对于任意一个完全二叉树来说，如果将含有的结点按照层次从左到右依次标号（如图 3a)），对于任意一个结点 i ，完全二叉树还有以下几个结论成立：
当 i>1 时，父亲结点为结点 [i/2] 。（i=1 时，表示的是根结点，无父亲结点）
如果 2*i>n（总结点的个数） ，则结点 i 肯定没有左孩子（为叶子结点）；否则其左孩子是结点 2*i 。
如果 2*i+1>n ，则结点 i 肯定没有右孩子；否则右孩子是结点 2*i+1 。


## 三、树的存储结构

### 1.二叉树的顺序存储
二叉树的顺序存储，指的是使用顺序表（数组）存储二叉树。**注：顺序存储只适用于完全二叉树。**

>  如果我们想顺序存储普通二叉树，需要提前将普通二叉树转化为完全二叉树。如下图所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0NjI0TTAyLTAucG5n?x-oss-process=image/format,png)

完全二叉树的顺序存储，仅需从根节点开始，按照层次依次将树中节点存储到数组即可。
比如上面的图2，存储结构如下：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0NjI0RjA0LTMucG5n?x-oss-process=image/format,png)
普通二叉树使用顺序表存储或多或多会存在空间浪费的现象，因此引入下面的链式存储。
### 2.二叉树的链式存储
二叉树链式存储结构示意图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0R0oyWi0xLmdpZg)
采用链式存储二叉树时，其节点结构由 3 部分构成（如图所示）：
指向左孩子节点的指针（Lchild）；
节点存储的数据（data）；
指向右孩子节点的指针（Rchild）；
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0R0swMzQtMi5naWY)
数据结构为：
```c
typedef struct BiTNode{
    TElemType data;//数据域
    struct BiTNode *lchild,*rchild;//左右孩子指针
    struct BiTNode *parent;
}BiTNode,*BiTree;
```
下面是一段完整的c语言代码
```c
#include <stdio.h>
#include <stdlib.h>
#define ELEMTYPE int

//Struct of binary tree node
typedef struct BiTNode {
	ELEMTYPE data;
	struct BiTNode *lchild, *rchild;//child node pointer
	struct BiTNode *parent;//parent node pointer
}BiTNode, *BiTree;

/**
*Function:	create a tree
*Input:		Bitree
*return
*/
void createTree(BiTree *T) {
	printf("Tree Graph:\n");
	printf("1:       1		\n");
	printf("        /\\		\n");
	printf("2:     2  3		\n");
	printf("      /			\n");
	printf("3:   4			\n");

	//Depth: 1
	*T = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->data = 1;

	//Depth: 2
	(*T)->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->data = 2;
	(*T)->lchild->parent = *T;

	(*T)->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->data = 3;
	(*T)->rchild->parent = *T;

	//Depth: 3
	(*T)->lchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->lchild->data = 4;
	(*T)->lchild->lchild->parent = (*T)->lchild;

	(*T)->lchild->rchild = NULL;
	(*T)->rchild->lchild = NULL;
	(*T)->rchild->rchild = NULL;

	//Depth: 4
	(*T)->lchild->lchild->lchild = NULL;
	(*T)->lchild->lchild->rchild = NULL;

}

int main() {
	BiTree tree;
	createTree(&tree);
	printf("Root is:%d\n",tree->data);
	printf("Parent of the fourth node is:%d\n", tree->lchild->lchild->parent->data);
	getchar();
	return 0;
}
```


**三叉链表**：在某些实际场景中，可能会做 "查找某节点的父节点" 的操作，这时可以在节点结构中再添加一个指针域，用于各个节点指向其父亲节点。

## 四、先序遍历
二叉树先序遍历的实现思想是：
1.访问根节点；
2.访问当前节点的左子树；
3.若当前节点无左子树，则访问当前节点的右子树；
如图所示二叉树：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0VDY0NEItMC5wbmc?x-oss-process=image/format,png)
### 递归方法：
二叉树的先序遍历采用的是递归的思想，因此可以递归实现，其 C 语言实现代码为（完整代码见非递归）：

```c
void Preorder(BiTree T) {
	if (T) {
		Visit(T);
		Preorder(T->lchild);
		Preorder(T->rchild);
	}
	return;
}
```

### 非递归方法：
而递归的底层实现依靠的是栈存储结构，因此，二叉树的先序遍历既可以直接采用递归思想实现，也可以使用栈的存储结构模拟递归的思想实现，思路如下：
1.对于每个节点判断是否有左右节点，
2.如果有左节点则继续访问左孩子节点，并判断其是否有右节点，如果有则入栈，没有则忽略。
3.如果当前节点没有左孩子节点，则继续判断栈是否有元素，如果有则回到栈节点继续访问右子树。如果没有则前序遍历完成。

其 C 语言实现代码为：

```c
#include <stdio.h>
#include <stdlib.h>
#define ELEMTYPE int
#define MAXSIZE 100

static int top = -1;
//Struct of binary tree node
typedef struct BiTNode {
	ELEMTYPE data;
	struct BiTNode *lchild, *rchild;//child node pointer
}BiTNode, *BiTree;

/**
*Function:	create a tree
*Input:		Bitree
*return
*/
void CreateTree(BiTree *T) {
	printf("Tree Graph:\n");
	printf("1:       1		\n");
	printf("        /\\		\n");
	printf("2:     2  3		\n");
	printf("      /\\  /\\ 	\n");
	printf("3:   4 5  6 7	\n");

	//Depth: 1
	*T = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->data = 1;

	//Depth: 2
	(*T)->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->data = 2;

	(*T)->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->data = 3;

	//Depth: 3
	(*T)->lchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->lchild->data = 4;

	(*T)->lchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->rchild->data = 5;

	(*T)->rchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->lchild->data = 6;

	(*T)->rchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->rchild->data = 7;

	//Depth: 4
	(*T)->lchild->lchild->lchild = NULL;
	(*T)->lchild->lchild->rchild = NULL;

	(*T)->lchild->rchild->lchild = NULL;
	(*T)->lchild->rchild->rchild = NULL;

	(*T)->rchild->lchild->lchild = NULL;
	(*T)->rchild->lchild->rchild = NULL;

	(*T)->rchild->rchild->lchild = NULL;
	(*T)->rchild->rchild->rchild = NULL;

}
/**
*Function:	visit a node
*Input:		BiTNode
*return
*/
void Visit(BiTNode *T) {
	printf("Preorder:%d\n", T->data);
}
/**
*Function:	preorder visit a tree
*Input:		Bitree
*return
*/
void Preorder(BiTree T) {
	if (T) {
		Visit(T);
		Preorder(T->lchild);
		Preorder(T->rchild);
	}
	return;
}
/**
*Function:	pop
*Input:		BiTNode*[]
*return		BiTNode*--The top element of stack.
*/
BiTNode* pop(BiTNode* Stack[]) {
	BiTNode* a = Stack[top--];
	return a;
}
/**
*Function:	push
*Input:		BiTNode*[], BiTNode*
*return
*/
void push(BiTNode* Stack[], BiTNode* a) {
	Stack[++top] = a;
}
/**
*Function:	preorder visit a tree by using a stack
*Input:		Bitree
*return
*/
void PreorderStack(BiTree T) {
	BiTNode* Stack[MAXSIZE];//Initialize the stack
	while (T) {
		Visit(T);
		if (T->lchild) {//If T has left child
			if (T->rchild) {	//If T has left and right child
				push(Stack,T->rchild);
			}
			T = T->lchild;
		}
		else if(T->rchild){//If T only has right child
			T = T->rchild;
		}
		else if(top>=0){//If Stack has elements
			BiTNode *a = pop(Stack);
			T = a;
		}
		else {//If T is last node in preorder.
			break;
		}
	}


	return;
}
int main() {
	BiTree tree;
	CreateTree(&tree);
	//recursive
	printf("Recursive PreOrder\n");
	Preorder(tree);

	//not recursive
	printf("Non-recursive PreOrder\n");
	PreorderStack(tree);
	getchar();
	return 0;
}
```


## 五、中序遍历
二叉树中序遍历的实现思想是：
1.访问当前节点的左子树；
2.访问根节点；
3.访问当前节点的右子树；

如图所示二叉树：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0VDY0NEItMC5wbmc?x-oss-process=image/format,png)
### 递归方法
二叉树的中序遍历采用的是递归的思想，因此可以递归实现，其 C 语言实现代码为（完整代码见非递归方法）：

```c
/**
*Function:	InOrder visit a tree
*Input:		Bitree
*return
*/
void InOrder(BiTree T) {
	if (T) {
		InOrder(T->lchild);
		Visit(T);
		InOrder(T->rchild);
	}
	return;
}
```
### 非递归方法
我的思路如下：
访问根节点，判断其是否有左右孩子。
如果有左孩子，则根节点入栈，继续访问左子树。如果左子树为空，则访问出栈元素，然后访问其右子树。
如果没有左孩子，则判断是否有右孩子，并访问当前节点。
```c
#include <stdio.h>
#include <stdlib.h>
#define ELEMTYPE int
#define MAXSIZE 100

static int top = -1;
//Struct of binary tree node
typedef struct BiTNode {
	ELEMTYPE data;
	struct BiTNode *lchild, *rchild;//child node pointer
}BiTNode, *BiTree;

/**
*Function:	create a tree
*Input:		Bitree
*return
*/
void CreateTree(BiTree *T) {
	printf("Tree Graph:\n");
	printf("1:       1		\n");
	printf("        /\\		\n");
	printf("2:     2  3		\n");
	printf("      /\\  /\\ 	\n");
	printf("3:   4 5  6 7	\n");

	//Depth: 1
	*T = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->data = 1;

	//Depth: 2
	(*T)->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->data = 2;

	(*T)->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->data = 3;

	//Depth: 3
	(*T)->lchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->lchild->data = 4;

	(*T)->lchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->rchild->data = 5;

	(*T)->rchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->lchild->data = 6;

	(*T)->rchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->rchild->data = 7;

	//Depth: 4
	(*T)->lchild->lchild->lchild = NULL;
	(*T)->lchild->lchild->rchild = NULL;

	(*T)->lchild->rchild->lchild = NULL;
	(*T)->lchild->rchild->rchild = NULL;

	(*T)->rchild->lchild->lchild = NULL;
	(*T)->rchild->lchild->rchild = NULL;

	(*T)->rchild->rchild->lchild = NULL;
	(*T)->rchild->rchild->rchild = NULL;

}
/**
*Function:	visit a node
*Input:		BiTNode
*return
*/
void Visit(BiTNode *T) {
	printf("InOrder:%d\n", T->data);
}
/**
*Function:	InOrder visit a tree
*Input:		Bitree
*return
*/
void InOrder(BiTree T) {
	if (T) {
		InOrder(T->lchild);
		Visit(T);
		InOrder(T->rchild);
	}
	return;
}
/**
*Function:	pop
*Input:		BiTNode*[]
*return		BiTNode*--The top element of stack.
*/
BiTNode* pop(BiTNode* Stack[]) {
	BiTNode* a = Stack[top--];
	return a;
}
/**
*Function:	push
*Input:		BiTNode*[], BiTNode*
*return
*/
void push(BiTNode* Stack[], BiTNode* a) {
	Stack[++top] = a;
}
/**
*Function:	InOrder visit a tree by using a stack
*Input:		Bitree
*return
*/
void InOrderStack(BiTree T) {
	BiTNode* Stack[MAXSIZE];//Initialize the stack
	while (T) {
		if (T->lchild) {//If T has left child
			push(Stack, T);
			T = T->lchild;
		}
		else {
			Visit(T);
			if (T->rchild) {//If T has right child
				T = T->rchild;
			}
			else if (top >= 0) {//If Stack has elements
				BiTNode *a = pop(Stack);
				Visit(a);
				T = a->rchild;
			}
			else {//If T is last node in preorder.
				break;
			}
		}
	}


	return;
}
int main() {
	BiTree tree;
	CreateTree(&tree);
	printf("Recursive InOrder\n");
	//recursive
	InOrder(tree);

	printf("Non-recursive InOrder\n");
	//not recursive
	InOrderStack(tree);
	getchar();
	return 0;
}
```

两种更标准的方法如下：
中序遍历的非递归方式实现思想是：从根结点开始，遍历左孩子同时压栈，当遍历结束，说明当前遍历的结点没有左孩子，从栈中取出来调用操作函数，然后访问该结点的右孩子，继续以上重复性的操作。

除此之外，还有另一种实现思想：中序遍历过程中，只需将每个结点的左子树压栈即可，右子树不需要压栈。当结点的左子树遍历完成后，只需要以栈顶结点的右孩子为根结点，继续循环遍历即可。


## 六、后序遍历

二叉树中序遍历的实现思想是：
1.访问当前节点的左子树；
2.访问当前节点的右子树；
3.访问根节点；

如图所示二叉树：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0VDY0NEItMC5wbmc?x-oss-process=image/format,png)
### 递归方法
二叉树的后序遍历采用的是递归的思想，因此可以递归实现，其 C 语言实现代码为（完整代码见非递归方法）：

```c
/**
*Function:	Recursive method to traverse a tree in post-order
*Input:		Bitree
*return
*/
void PostOrder(const BiTree T) {
	if (T) {
		PostOrder(T->lchild);
		PostOrder(T->rchild);
		Visit(T);
	}
	return;
}
```
### 非递归方法
非递归算法的思路是：
后序遍历的逆序 是 先序遍历交换左右子树遍历顺序。
因此用两个栈来实现。

```c
#include <stdio.h>
#include <stdlib.h>
#define ELEMTYPE int
#define MAXSIZE 100

static int top = -1;   //index of stack
static int printtop = -1; //index of printstack
//Struct of binary tree node
typedef struct BiTNode {
	ELEMTYPE data;
	struct BiTNode *lchild, *rchild;//child node pointer
}BiTNode, *BiTree;

/**
*Function:	create a tree
*Input:		Bitree
*return
*/
void CreateTree(BiTree *T) {
	printf("Tree Graph:\n");
	printf("1:       1		\n");
	printf("        /\\		\n");
	printf("2:     2  3		\n");
	printf("      /\\  /\\ 	\n");
	printf("3:   4 5  6 7	\n");

	//Depth: 1
	*T = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->data = 1;

	//Depth: 2
	(*T)->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->data = 2;

	(*T)->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->data = 3;

	//Depth: 3
	(*T)->lchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->lchild->data = 4;

	(*T)->lchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->rchild->data = 5;

	(*T)->rchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->lchild->data = 6;

	(*T)->rchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->rchild->data = 7;

	//Depth: 4
	(*T)->lchild->lchild->lchild = NULL;
	(*T)->lchild->lchild->rchild = NULL;

	(*T)->lchild->rchild->lchild = NULL;
	(*T)->lchild->rchild->rchild = NULL;

	(*T)->rchild->lchild->lchild = NULL;
	(*T)->rchild->lchild->rchild = NULL;

	(*T)->rchild->rchild->lchild = NULL;
	(*T)->rchild->rchild->rchild = NULL;

}
/**
*Function:	visit a node
*Input:		BiTNode
*return
*/
void Visit(BiTNode *T) {
	printf("PostOrder:%d\n", T->data);
}
/**
*Function:	Recursive method to traverse a tree in post-order
*Input:		Bitree
*return
*/
void PostOrder(const BiTree T) {
	if (T) {
		PostOrder(T->lchild);
		PostOrder(T->rchild);
		Visit(T);
	}
	return;
}
/**
*Function:	pop
*Input:		BiTNode*[]
*return		BiTNode*--The top element of stack.
*/
BiTNode* pop(BiTNode* Stack[]) {
	BiTNode* a = Stack[top--];
	return a;
}
/**
*Function:	push
*Input:		BiTNode*[], BiTNode*
*return
*/
void push(BiTNode* Stack[], BiTNode* a) {
	Stack[++top] = a;
}
/**
*Function:	printpop for printstack
*Input:		BiTNode*[]
*return		BiTNode*--The top element of stack.
*/
BiTNode* printpop(BiTNode* Stack[]) {
	BiTNode* a = Stack[printtop--];
	return a;
}
/**
*Function:	printpush for printstack
*Input:		BiTNode*[], BiTNode*
*return
*/
void printpush(BiTNode* Stack[], BiTNode* a) {
	Stack[++printtop] = a;
}
/**
*Function:	PostOrder visit a tree by using two stacks
*Input:		Bitree
*return
*/
void PostOrderStack(BiTree T) {
	BiTNode* Stack[MAXSIZE];//Initialize the stack
	BiTNode* PrintStack[MAXSIZE];//Initialize the stack
	while (T) {
		printpush(PrintStack, T);
		if (T->rchild) {//If T has left child
			if (T->lchild) {	//If T has left and right child
				push(Stack, T->lchild);
			}
			T = T->rchild;
		}
		else if (T->lchild) {//If T only has right child
			T = T->lchild;
		}
		else if (top >= 0) {//If Stack has elements
			BiTNode *a = pop(Stack);
			T = a;
		}
		else {//If T is last node in preorder.
			break;
		}
	}

	while (printtop >= 0) {
		Visit(printpop(PrintStack));
	}

	return;
}

int main() {
	BiTree tree;
	CreateTree(&tree);
	printf("Recursive method\n");
	//Recursive method to traverse a tree in post-order
	PostOrder(tree);

	printf("Non-recursive method\n");
	//Non-recursive method to traverse a tree in post-order
	PostOrderStack(tree);
	getchar();
	delete tree;
	return 0;
}
```
另一种算法思路：
用一个栈，但是设置标志位。
后序遍历是在遍历完当前结点的左右孩子之后，才调用操作函数，所以需要在操作结点进栈时，为每个结点配备一个标志位。当遍历该结点的左孩子时，设置当前结点的标志位为 0，进栈；当要遍历该结点的右孩子时，设置当前结点的标志位为 1，进栈。

这样，当遍历完成，该结点弹栈时，查看该结点的标志位的值：如果是 0，表示该结点的右孩子还没有遍历；反之如果是 1，说明该结点的左右孩子都遍历完成，可以调用操作函数。
[代码实现](http://c.biancheng.net/view/3390.html)
## 七、层次遍历
按照二叉树中的层次从左到右依次遍历每层中的结点。
具体的实现思路是：通过使用队列的数据结构，从树的根结点开始，依次将其左孩子和右孩子入队。而后每次队列中一个结点出队，都将其左孩子和右孩子入队，直到树中所有结点都出队，出队结点的先后顺序就是层次遍历的最终结果。

如图1所示二叉树：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk0VDY0NEItMC5wbmc?x-oss-process=image/format,png)
例如，层次遍历图 1 中的二叉树：
首先，根结点 1 入队；
根结点 1 出队，出队的同时，将左孩子 2 和右孩子 3 分别入队；
队头结点 2 出队，出队的同时，将结点 2 的左孩子 4 和右孩子 5 依次入队；
队头结点 3 出队，出队的同时，将结点 3 的左孩子 6 和右孩子 7 依次入队；
不断地循环，直至队列内为空。
```c
#include <stdio.h>
#include <stdlib.h>
#define ELEMTYPE int
#define MAXSIZE 100

static int front = 0, rear = 0;
//Struct of binary tree node
typedef struct BiTNode {
	ELEMTYPE data;
	struct BiTNode *lchild, *rchild;//child node pointer
}BiTNode, *BiTree;

/**
*Function:	create a tree
*Input:		Bitree
*return
*/
void CreateTree(BiTree *T) {
	printf("Tree Graph:\n");
	printf("1:       1		\n");
	printf("        /\\		\n");
	printf("2:     2  3		\n");
	printf("      /\\  /\\ 	\n");
	printf("3:   4 5  6 7	\n");

	//Depth: 1
	*T = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->data = 1;

	//Depth: 2
	(*T)->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->data = 2;

	(*T)->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->data = 3;

	//Depth: 3
	(*T)->lchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->lchild->data = 4;

	(*T)->lchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->lchild->rchild->data = 5;

	(*T)->rchild->lchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->lchild->data = 6;

	(*T)->rchild->rchild = (BiTNode*)malloc(sizeof(BiTNode));
	(*T)->rchild->rchild->data = 7;

	//Depth: 4
	(*T)->lchild->lchild->lchild = NULL;
	(*T)->lchild->lchild->rchild = NULL;

	(*T)->lchild->rchild->lchild = NULL;
	(*T)->lchild->rchild->rchild = NULL;

	(*T)->rchild->lchild->lchild = NULL;
	(*T)->rchild->lchild->rchild = NULL;

	(*T)->rchild->rchild->lchild = NULL;
	(*T)->rchild->rchild->rchild = NULL;

}
/**
*Function:	visit a node
*Input:		BiTNode
*return
*/
void Visit(BiTNode *T) {
	printf("Hierarchical traversal:%d\n", T->data);
}

void EnQueue(BiTNode * queue[],BiTNode * a) {
	queue[rear++] = a;
}
BiTNode * DeQueue(BiTNode * queue[]) {
	BiTNode *b = queue[front++];
	return b;
}
/**
*Function:	Hierarchical traversal
*Input:		Bitree
*return
*/
void Hierarchical(const BiTree T) {
	BiTNode* Queue[MAXSIZE];
	BiTree q = T;
	EnQueue(Queue,q);
	while (front<rear) {
		BiTNode* node = DeQueue(Queue);
		Visit(node);
		if (node->lchild!=NULL) {
			EnQueue(Queue, node->lchild);
		}
		if (node->rchild != NULL) {
			EnQueue(Queue, node->rchild);
		}
	}
	return;
}

int main() {
	BiTree tree;
	CreateTree(&tree);
	//Hierarchical traversal
	Hierarchical(tree);
	getchar();
	delete tree;
	return 0;
}
```

## 八、哈夫曼树
### 基本概念
路径：在一棵树中，一个结点到另一个结点之间的通路，称为路径。图 1 中，从根结点到结点 a 之间的通路就是一条路径。

路径长度：在一条路径中，每经过一个结点，路径长度都要加 1 。例如在一棵树中，规定根结点所在层数为1层，那么从根结点到第 i 层结点的路径长度为 i - 1 。图 1 中从根结点到结点 c 的路径长度为 3。

结点的权：给每一个结点赋予一个新的数值，被称为这个结点的权。例如，图 1 中结点 a 的权为 7，结点 b 的权为 5。

结点的带权路径长度：指的是从根结点到该结点之间的路径长度与该结点的权的乘积。例如，图 1 中结点 b 的带权路径长度为 2 * 5 = 10 

![在这里插入图片描述](https://img-blog.csdn.net/20131205224108125?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3RmbW9ua2luZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

哈夫曼树：又称最优二叉树。它是 n 个带权叶子结点构成的所有二叉树中，带权路径长度 WPL 最小的二叉树。
如下图：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk1NjNUYjAtMC5wbmc?x-oss-process=image/format,png)
### 构建过程
对于给定的有各自权值的 n 个结点，构建哈夫曼树有一个行之有效的办法：
在 n 个权值中选出两个最小的权值，对应的两个结点组成一个新的二叉树，且新二叉树的根结点的权值为左右孩子权值的和；
在原有的 n 个权值中删除那两个最小的权值，同时将新的权值加入到 n–2 个权值的行列中，以此类推；
重复 1 和 2 ，直到所以的结点构建成了一棵二叉树为止，这棵树就是哈夫曼树。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2MuYmlhbmNoZW5nLm5ldC91cGxvYWRzL2FsbGltZy8xOTA0MjcvMDk1NjNRUzUtMS5wbmc?x-oss-process=image/format,png)
### 哈弗曼树中结点结构
构建哈夫曼树时，首先需要确定树中结点的构成。由于哈夫曼树的构建是从叶子结点开始，不断地构建新的父结点，直至树根，所以结点中应包含指向父结点的指针。但是在使用哈夫曼树时是从树根开始，根据需求遍历树中的结点，因此每个结点需要有指向其左孩子和右孩子的指针。

所以，哈夫曼树中结点构成用代码表示为：

```c
//Struct of huffman tree
typedef struct BTNode {
	ELEMTYPE data;
	struct BTNode *left, *right;//child node pointer
	struct BTNode *parent;//child node pointer
}BTNode, *HFTree;
```
### 构建哈弗曼树的算法实现及WPL的计算
构建哈夫曼树时，需要每次根据各个结点的权重值，筛选出其中值最小的两个结点，然后构建二叉树。

大佬的思路：查找权重值最小的两个结点的思想是：从树组起始位置开始，首先找到两个无父结点的结点（说明还未使用其构建成树），然后和后续无父结点的结点依次做比较，有两种情况需要考虑：
如果比两个结点中较小的那个还小，就保留这个结点，删除原来较大的结点；
如果介于两个结点权重值之间，替换原来较大的结点；
参考：[哈夫曼树c语言实现](https://blog.csdn.net/wtfmonking/article/details/17150499)
我的思路：
建立一个哈夫曼树：
创建工作指针p+tmp，
对于一组数，依次找出最小的数加入树中(选择排序思想)，
如果树没有创建，则创建一个结点存入第一个数。工作指针指向这个结点
如果工作指针没有左孩子，则工作指针左孩子指向下一个结点
如果工作指针没有右孩子，则工作指针右孩子指向下一个结点
如果工作指针有两个孩子，则创建一个空结点，左孩子指向工作指针，右孩子指向下一个结点，并将工作指针指向该结点。
计算wpl:
递归计算，
如果结点没有左右孩子则为叶子结点，返回权值*高度.
如果有孩子，则返回左子树wpl和右子树wpl之和.
```c
#include <stdio.h>
#include <stdlib.h>
#define ELEMTYPE int
#define MAXSIZE 100

static int top = -1;
//Struct of huffman tree
typedef struct BTNode {
	ELEMTYPE data;
	struct BTNode *left, *right;//child node pointer
	struct BTNode *parent;//child node pointer
}BTNode, *HFTree;

/**
*Function:	create a huffman tree
*Input:		int *nums--numbers to create tree,int n--num[] size
*return
*/
HFTree CreateTree(int *nums,int n) {
	BTNode *p_tmp =(BTNode*)malloc(sizeof(BTNode)); //work pointer
	p_tmp->left = NULL;
	p_tmp->right = NULL;
	p_tmp->parent = NULL;

	int lastnum = 0;//Initilize with a num less than the min num
	for (int i = 0; i < n; i++) {
		int min = 100;//Initilize with a num larger than the max num
		for (int j = 0; j < n; j++) {
			if (min>nums[j] && lastnum<nums[j]) {
				min = nums[j];
			}
		}
		lastnum = min;
		//create a new node
		BTNode *p_newchild= (BTNode*)malloc(sizeof(BTNode));
		p_newchild->data = min;
		p_newchild->left = NULL;
		p_newchild->right = NULL;
		p_newchild->parent = NULL;

		if (1 == n) {
			return p_newchild;
		}
		if (p_tmp->left==NULL) { //add node to left of p_tmp
			p_tmp->left = p_newchild;
			p_newchild->parent = p_tmp;
		}
		else if(p_tmp->right == NULL){//add node to right of p_tmp
			p_tmp->right = p_newchild;
			p_tmp->data = p_tmp->left->data + p_tmp->right->data;
			p_newchild->parent =p_tmp;
		}
		else {//add brother node of p_tmp and create p_newparent
			BTNode *p_newparent = (BTNode*)malloc(sizeof(BTNode));
			p_newparent->data = p_tmp->data+min;
			if (p_tmp->data<=min) {
				p_newparent->left = p_tmp;
				p_newparent->right = p_newchild;
			}
			else {
				p_newparent->left = p_newchild;
				p_newparent->right = p_tmp;
			}
			p_newparent->parent = NULL;
			p_newchild->parent = p_newparent;
			p_tmp = p_newparent;
		}
	}
	return p_tmp;
}
/**
*Function:	visit a node
*Input:		BiTNode
*return
*/
void Visit(BTNode *T) {
	printf("PreOrder:%d\n", T->data);
}
/**
*Function:	PreOrder visit a tree
*Input:		Bitree
*return
*/
void PreOrder(HFTree T) {
	if (T) {
		Visit(T);
		PreOrder(T->left);
		PreOrder(T->right);
	}
	return;
}
/**
*Function:	calculate the weight path length of HFTree
*Input:		HFTree T, int len--depth
*return		wpl
*/
int cal_wpl(HFTree T, int len) {
	if (T) {
		//If leaf node
		if (T->left==NULL && T->right==NULL) {
			return T->data*len;
		}
		else {
			//If not leaf node, return left child tree wpl and right child tree wpl
			return cal_wpl(T->left,len+1) + cal_wpl(T->left,len+1);
		}
	}
	//Null tree return 0;
	return 0;
}
int main() {
	
	int nums[5] = {9,5,1,2,18};
	HFTree tree=CreateTree(nums,5);

	//recursive visit tree node
	PreOrder(tree);
	printf("%d",cal_wpl(tree,0));
	getchar();
	return 0;
}
```
