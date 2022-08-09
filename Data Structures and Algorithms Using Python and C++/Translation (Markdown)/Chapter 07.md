# 树

目标

* 学习树数据结构的相关术语。

* 了解树数据结构适用于的各种应用程序。

* 能够使用链接或者数组来实现树结构，并且熟悉基于树的基本算法。

* 了解二叉搜索树结构以及它的各种操作的效率。

* 通过更多的练习来提高对递归算法的理解。

## 概要

到目前为止，我们主要处理的都是像列表、堆栈和队列这样的线性数据结构，它们一般被用来表示序列中的各个元素。
在本章里，我们将与之前讲的“分一个岔”，来考虑一个被称为*树*（*Tree*）的非线性数据结构。
树是按照层级的方式来存储数据的，因此，这让它非常便于对现实世界的层次结构进行建模。
例如，你肯定对表示亲属信息的家谱的概念非常熟悉。
其他一些关于树的例子有：
分类学以及公司的汇报结构。

比如说，我们可以使用树来表示生物学家使用的生物类群里的动物。
动物可以细分为脊椎动物和无脊椎动物；
脊椎动物可以细分为爬行动物、鱼类、哺乳动物等等。
这棵树看起来就会像是图7.1这样的。
层次关系在我们的生活中随处可见，因此，树在许多的应用程序里都被用来作为数据的自然表示。

图7.1：生物学家的生物类群的一部分

可能会让你惊讶的是，事实证明，在实现之前提到的普通的序列数据的时候，树也非常有用。
在这一章里，我们将看到被称为*二叉搜索树*（*binary search trees*）的树结构。
它被用来实现一个允许高效插入和删除（类似于链表）的集合，但它也同时能够进行高效的搜索（类似于有序数组）。
基于树的数据结构和算法对于高效处理大量的数据（如数据库和文件系统）来说至关重要。

## 树的术语

计算机科学家们用一个包含节点的集合（类似于链表中的节点）以及用来连接它们的*边*（*edge*）来表示树。
图7.2显示了一个包含七个节点的树，其中每个节点都包含着一个整数。
图的最顶端的节点被称为*根*（*root*）。
在这个树里，根包含数据值是`2`。
一个树只能有一个根。
因此，你可以通过跟随从根开始的边（箭头）来到达树里面的任何一个其他节点。

图7.2：二叉树示例

树里的每个节点都可以通过边与它的*子节点*（*child*）相连接。
在一颗普通的树里，一个节点可以有任意数量的子节点，但在这里，让我们先只关注*二叉树*（*binary trees*）。
在二叉树里，每个节点最多只能有两个子节点。
就像你看到的图7.2那样，这里所描绘的树就是二叉树。
树里面的关系，使用的是把家庭和树相关的术语的混合起来描述的。
像这样，根节点有两个子节点：
包含`7`的节点是它的*左子节点*（*left child*）；
包含`6`的节点是它的*右子节点*（*right child*）。
这两个节点也被称为*兄弟节点*（*sibling*）。
同样的，包含`8`和`4`的两个节点就也是兄弟节点。
那么节点`5`的*父节点*（*parent*）是节点`7`。
同样的有节点`3`是节点`7`的*后代*（*descendant*），节点`7`是节点`3`的*祖先*（*ancestor*）。
最后，没有任何子节点的节点则被叫做*叶节点*（*leaf*）。
节点的*深度*（*depth*）代表它与根节点之间的边数。
对于根节点来说，它的深度为零。
节点`7`和`6`的深度为`1`，节点`3`的深度则为`3`。
*树的高度*（*height*）或者说*树的深度*（*depth*）是所有节点里的最大深度。

在*满二叉树*（*full binary tree*）中，每个深度级别的每一个可能的位置都有一个节点。
在最下面一层，所有的节点都是叶节点（也就是说，所有的叶节点都处于相同的深度，并且每个非叶节点都具有两个子节点）。
而完全二叉树是，在除了最深层之外的每一个可能位置都有一个节点。
并且在最深的那一层，节点按照从左到右的位置进行排列。
可以通过从满二叉树开始，然后在下一层从左到右添加节点，或者是从右到左删除最后一层的节点来创建完全二叉树。
对于它们两个的示例，可以参考图7.3。

图7.3：左侧是满二叉树，右侧是完全二叉树

树的每个节点及其后代都可以被视为一棵*子树*（*subtree*）。
比如说，在图7.2里，节点`7`、`5`和`3`组合在一起可以被认为是整个树的一棵子树，这其中，节点`7`是这个子树的根节点。
通过这种方式来看的话，很明显的，树可以被当作递归结构。
一棵二叉树可以是空树，也可以是根节点和（可能为空的）左右子树组合起来的。

和列表类似的，树的一个非常有用的操作是遍历。
给定一棵树，我们需要有一种合适的方式来系统地“走过”这颗树的每一个节点。
但是和列表不同的是，并没有一个很清晰的遍历树的方法。
可以看到，树里的每一个节点都由三部分组成：数据，左子树和右子树。
因此，根据我们的侧重点来决定处理这些数据，我们可以有三种不同的遍历顺序来进行选择。
如果我们先在根节点处理数据，然后再去处理左右子树的话，我们将会执行被称为*前序遍历*（*preorder traversal*）的遍历方法。
这个遍历方法之所以叫这个名字，是因为我们首先需要考虑的是根节点里的数据。
前序遍历可以很容易地被表示成一个递归算法：

```Python
def traverse(tree):
    if tree is not empty:
        process data at tree’s root     # preorder traversal
        traverse(tree’s left subtree)
        traverse(tree’s right subtree)
```

把这个算法应用于图7.2里的树的话，节点将会按照`2`，`7`，`5`，`3`，`6`，`8`，`4`这样的顺序进行处理。

当然，我们也可以通过简单的移动实际处理数据的位置来修改遍历算法。
*中序遍历*（*inorder traversal*）将会在处理两个子树之间的时候处理根节点的数据。
对于我们那个例子里的树，中序遍历将会按照`7`，`3`，`5`，`2`，`8`，`6`，`4`的序列来处理节点。
你现在可能已经猜到了，*后序遍历*（*postorder traversal*）会在处理完两个子树之后再去处理根节点，这就给了我们这样一个顺序：`3`，`5`，`7`，`8`，`4`，`6`，`2`。

## 示例应用程序：表达式树

树在计算机科学中的一个重要应用是存储程序的内部结构。
当解释器或编译器分析程序的时候，它会去构造一个用来提取程序结构的*解析树*（*parse tree*）。
例如，考虑这样一个简单的表达式：`(2 + 3) * 4 + 5 * 6`。
这个表达式可以用图7.4中的树的形式来表现。
可以仔细看看，树的层次结构是怎么消除了对括号的需要的。
表达式的基本操作数将会是树的叶节点，而运算符将会是树的内部节点。
在树的低层级里的操作将会必须要被优先执行，只有这样它的结果才能够被用在更高层级的表达式里。
很明显，加法`2 + 3`将必须是第一个需要执行的操作，这是因为它出现在了树的最低层。

图7.4：数学表达式的树呈现

将表达式表现为树结构之后，我们就能够做很多有趣的事情了。
编译器将遍历这个结构来生成执行计算的一系列的机器指令。
解释器也会使用这个结构来执行这个表达式。
它可以通过获取两个子节点的值，再使用这个操作来计算出每个节点的值。
如果其中的一个或两个子节点本身是一个运算符，那么就必须要先对这个子节点进行计算。
一个简单的树的后序遍历就能够被用来计算表达式。

```Python
def evaluateTree(tree):
    if tree’s root is an operand:
        return root data
    else:   # root contains an operator
        leftValue = evaluateTree(tree’s left subtree)
        rightValue = evaluateTree(tree’s right subtree)
        result = apply operator at root to leftValue and rightValue
        return result
```

如果你够仔细的话，你会发现这个算法其实是一个——用来计算表达式的后缀版本的——递归算法。
简单地按照后序遍历这个表达式树会产生这个序列：`2 3 + 4 * 5 6 * +`，而这正好是我们最初的表达式的后缀表示法。
在第5章里，我们用了一个堆栈的算法来计算后缀方程。
在这里，通过利用递归会隐式地使用计算机的运行时堆栈，我们也完成了相同的任务。
顺便说一句，你也可以通过执行恰当的遍历，来获取表达式的前缀版本和中缀版本。
当这一切的知识都相互交织在一起的时候，难道不令人着迷吗？

## 树的存储方式

现在，你已经了解了树可以做什么，接下来考我们虑一下树的可能的具体存储方式。
构建树的一种简单明了的方法是使用链接来表示。
和我们处理链表一样，我们可以创建一个类来表示树的节点。
每个节点都有一个实例变量来保存对节点数据的引用，同时还有用来引用左右子节点的变量。
我们将使用`None`对象来表示空子树。
于是，我们有了这样一个Python的类：

```python
# TreeNode.py
class TreeNode(object):
    def __init__(self, data = None, left=None, right=None):
        """creates a tree node with specified data and references to left
        and right children"""
        self.item = data
        self.left = left
        self.right = right
```

通过使用这个`TreeNode`类，就能够很方便地直接像镜像一样创建我们已经看到过的二叉树图的链式结构。
比如，下面这段代码就可以构建一个包含有三个节点的简单树：

```Python
left = TreeNode(1)
right = TreeNode(3)
root = TreeNode(2, left, right)
```

通过简单地对`TreeNode`类的构造函数的组合调用，我们也可以用一行代码来达到相同的目的。

```Python
root = TreeNode(2, TreeNode(1), TreeNode(3))
```

更进一步地，我们甚至可以利用这种方法来创建各个树的节点，从而构建出任意复杂度的树结构。
下面这段代码可以创建出类似于图7.2这样的树结构。

```Python
root = TreeNode(2,
           TreeNode(7,
               None,
               TreeNode(5,
                   TreeNode(3),
                   None
               )
           )
           TreeNode(6,
               TreeNode(8),
               TreeNode(4)
           )
       )
```

在这里，通过使用缩进来帮助我们直观地让表达式的布局与树的结构相匹配。
比如说，从例子里我们可以看到，在根节点（`2`）的下面，有两个具有缩进的子树（`7`和`6`）。
如果你觉得这样并不是很直观的话，可以试着把头侧着看这段代码。

当然，通常来说，我们并不希望像这样通过直接操作`TreeNodes`类来构建复杂结构。
与之相反的是，我们一般会创建一个更高级别的容器类，通过这个容器类来封装树的构建的细节，并且提供了一组方便的API来操作树结构。
容器类的具体设计将取决于我们需要使用树去完成的任务。
在下一节里，我们将会看到这方面的例子。

我们应该提到过了，显而易见的，可以用链接来存储树。
但是，这并不是二叉树的唯一的实现方式。
在某些情况下，使用基于数组/列表的方法来实现树结构也很方便。
因为，我们可以通过数组中的位置来隐式地维护节点之间的关系，而不是用显式的链接来存储子节点。

在通过数组来实现树的方案里，我们将会先假设我们总是有一颗完整的树。
然后，我们可以逐级地将节点放置到数组里去。
所以，数组中的第一个单元格将会存储根节点，后面的两个位置将会用来存储根节点的子节点，再接下来的四个位置用来存储孙节点，后面的也依此类推。
按照这种方法，对于位置$i$的节点，总是有：它的左子节点位于位置$2 * i + 1$，它的右子节点将会位于位置$2 * i + 2$。
节点$i$的父节点会位于$(i - 1) // 2$。
在这里有一点需要注意的是，对于这些公式来说，每个节点都始终有两个子节点这个假设将会非常重要。
为了满足这个假设，你将需要用一些特殊的标记值（例如`None`）来表示空节点。
对于图7.2中的示例二叉树，将会被存储为这样的数组：`[2, 7, 6, None, 5, 8, 4, None, None, 3]`。
如果你想计算简单一点，你可以把数组里的第一个位置（索引`0`）留空，然后把根节点放在索引`1`的位置。
这样的话，对于位置$i$的节点，左子节点将会位于$2 * i$，右子节点则会位于$2 * i + 1$，而父节点将会在位置$i // 2$。

树的基于数组的实现方法的优点是：它不需要使用内存来显式地存储到子节点的链接。
但是，它却需要我们为空节点浪费单元格。
如果树是非常的稀疏的话，数组/列表里将会将会有大量的`None`元素，而且我们曾经提到过，数组/列表的实现也并不能有效地利用内存。
因此，基于这些问题，通过链接来实现树，将会更加合适。

## 应用：二叉搜索树

在这一节里，我们将会为有序序列构建另一个容器类，通过创建这个容器类，我们会了解到树的一种实现技术。
曾经在4.7节里，我们讨论过应该如何权衡序列的链接或者是数组的实现。
虽然链表提供了高效的插入和删除操作（因为不用移动元素），但它们不具备高效的搜索操作。
而同时，一个有序的数组将能够提供高效的搜索操作（通过二分搜索算法），但是插入和删除操作将会需要$Θ(n)$时间。
然而在这里，通过使用一种特殊结构的树——二叉搜索树——我们将能够结合这两者的优点。

### 二分查找属性

*二叉搜索树*（*binary search tree*）只是一个二叉树，但是这个树中的每一个节点都具有这样一个额外的属性：
任意节点的左子树里的值都将小于节点上的值，而它的右子树里的值则会大于节点上的值。
图7.5就显示了一个二叉搜索树的例子。

图7.5：二叉搜索树的例子

在一个二叉搜索树里搜索元素的话，一般来说，将会非常的高效。
我们将会先从树的根节点开始，并且检查这个节点的数据值。
如果根节点的值就是我们要查找的值，那么我们就完成整个搜索。
如果我们搜索的值小于根节点的值，那么我们就能知道，如果这个值存在于树里的话，那么它只可能在左子树里。
类似的，如果我们要搜索的值大于根节点的值，那说明它应该在右子树里。
我们可以到相对应的子树里，用相同的规则来继续这个搜索过程。
直到我们找到了这个元素，或者会找到一个空子树的节点说明二叉搜索树里并没有这个值，而如果要插入这个值进二叉搜索树的话，这个节点正好是这个值所会在的位置。
如果这个树相当“平衡”的话，那么对于每个节点来说，我们基本上能够做到，将必须要进行比较的元素的数量减少了一半。
换句话说，我们正在执行二分搜索算法，这也就是为什么它被称为二叉搜索树的原因。

### 实现一个二叉搜索树

遵循良好的设计原则，我们将编写一个`BST`（Binary Search Tree）类，这个类将会被用来封装二叉搜索树的所有细节，并且提供一组易于使用的接口。
我们的树结构将会维护一组元素，并且允许我们进行添加，删除和搜索特定值的操作。
在这里，我们将会使用链接来练习引用相关的知识，当然你也可以很简单地把它转换为之前我们讨论过的基于数组的实现。
`BST`对象将会包含对`TreeNode`对象的引用，这个`TreeNode`对象的引用是二叉搜索树的根节点。
在最初的时候，树是空树。
因此，这个引用将会是`None`。
于是，有了我们的类的构造函数。

```Python
# BST.py
from TreeNode import TreeNode

class BST(object):

    def __init__(self):
        """create empty binary search tree
        post: empty tree created"""

        self.root = None
```

现在，让我们来解决把元素添加到二叉搜索树这个问题。
一次只添加一个叶节点来生成一棵树很容易实现。
这个实现的一个关键点是，在给定的现有的二叉搜索树里，有且只有一个位置可以被用来放进新插入的元素。
让我们来考虑一个例子。
假设我们想在图7.6所展示的二叉搜索树中插入`5`。
那么，从根节点`6`开始的话，我们可以知道`5`必须也只能进入左子树。
这颗左子树的根的值是`2`，所以我们继续进入到它的右子树。
这颗子树的根具有的值是`4`，因此我们将继续进入到它的右子树。
而这个时候，这颗右子树是空的。
也就是说，应该在这个地方插入`5`作为新的叶节点。

图7.6：插入二叉搜索树的示例

我们可以使用迭代或者递归的方法来实现这个基本的插入算法。
无论使用哪种方式，我们都会从树的顶部开始，不断地向下执行，并且根据需要来决定是去左子树或者是右子树，直到找到新元素应该存放的位置。
和其他链式结构的算法相同的是，我们需要在整个结构为空的时候，去特别注意一下特殊情况。
这是因为，在这种情况下，相关的操作需要我们去更改根节点的实例变量。
这是算法的一个版本，它使用循环来向下遍历整个树结构。

```Python
    def insert(self, item):
        """insert item into binary search tree
        pre: item is not in self
        post: item has been added to self"""

        if self.root is None:   # handle empty tree case
            self.root = TreeNode(item)
        else:
            # start at root
            node = self.root
            # loop to find the correct spot (break to exit)
            while True:
                if item == node.item:
                    raise ValueError("Inserting duplicate item")

                if item < node.item:    # item goes in left subtree
                    if node.left is not None: # follow existing subtree
                        node = node.left
                    else:               # empty subtree, insert here
                        node.left = TreeNode(item)
                        break
                else:                   # item goes in right subtree
                    if node.right is not None: # follow existing subtree
                        node = node.right
                    else:               # empty subtree, insert here
                        node.right = TreeNode(item)
                        break
```

这段代码鉴于它的嵌套决策结构，看起来相当的复杂。
但是，跟踪代码的话，你应该不会有太多的麻烦。
代码里需要注意的是，我们有一个保证这个元素没有在这颗树里存在的这个先验条件。
一个纯粹二叉搜索树是不允许出现一个值的多个副本的，因此我们会去检查这个条件，在这个树结构里如果已经存在了相同的元素的时候就抛出异常。
我们假如想要扩展这个设计，让它允许出现多个值的话，只需要轻松地通过在每个节点中都保留已添加的值的次数就可以了。

随着这个算法的出现，为了能够让你记忆得更清晰，我们还可以再要考虑一下，应该如何使用递归来解决这个问题。
我们在上面曾经说过，树结构是一种自然递归的数据结构，但是我们的`BST`类并不是一个真正的递归结构。
但是，树的互相链接节点本身的结构是递归的。
因此，我们可以认为树里的任何一个节点都是它的子树的根，并且，它本身会包含两个更小的子树。
当然，`None`值代表着这个子树为空。
有了这样的点子，我们就能够很容易地将插入算法转换为对子树进行操作的递归方法。
我们将会按照这个设计，写一个递归的辅助方法。
通过调用这个辅助方法来执行插入操作。
这样的话，插入方法本身将会非常的小。

```Python
def insert_rec(self, item):

    """insert item into binary search tree
    pre: item is not in self
    post: item has been added to self"""

    self.root = self._subtreeInsert(self.root, item)
```

清楚地了解`_subtreeInsert`做了什么是非常重要的。
可以看到，这个方法将会接收一个节点和需要被插入的元素（`item`）；
同时，这个节点是将会被插入的元素所在的子树的根节点。
在一开始的情况下，这个节点是整个树结构（`self.root`）。
`_subtreeInsert`将会同时包含执行插入的操作，以及会返回可以被用来当作结果的（子）树的根节点。
这种方法能够确保我们的插入（`insert`）操作即使面对的是最初的空树也能工作。
因为，在这种情况下，`self.root`在开始的时候是`None`（表示空树），而在`_subtreeInsert`方法返回了包含这个元素（`item`）的`TreeNode`之后，这个节点就会成为这颗树的新的根节点。

现在，让我们来编写递归的辅助函数`_subtreeInsert`吧。
函数的参数为我们提供了元素需要被插入的树结构的根节点，在最后，这个函数还需要返回结果树的根节点。
整个算法非常的简单：
如果这个（子）树是空的，我们只需返回包含这个元素的`TreeNode`就行了。
而如果这颗树不为空的话，我们通过递归地把这个元素添加到（相应的）左子树或者右子树里就行了，然后返回这颗树的根节点来作为新树的根系欸但（因为，这个节点没有被改变）。
下面是完成这些相应工作的代码：

```Python
    def _subtreeInsert(self, root, item):
        if root is None:            # inserting into empty tree
            return TreeNode(item)   # the item becomes the new tree root

        if item == root.item:
            raise ValueError("Inserting duplicate item")

        if item < root.item:                # modify left subtree
            root.left = self._subtreeInsert(root.left, item)
        else:                               # modify right subtree
            root.right = self._subtreeInsert(root.right, item)

        return root # original root is root of modified tree
```

到目前为止，我们可以创建一个`BST`对象并且添加元素到这个对象里了。
因此，我们能够使用某种方法来查找树里的元素了。
我们曾经讨论过了的基本的搜索算法，在这个算法里应该能够很容易地实现。
因为它只需要一个循环来从根节点向下遍历整个树，直到找到这个目标元素或者我们到达了整个树的底部为止：

```Python
    def find(self, item):

        """ Search for item in BST
        post: Returns item from BST if found, None otherwise"""

        node = self.root
        while node is not None and not(node.item == item):
            if item < node.item:
                node = node.left
            else:
                node = node.right

        if node is None:
            return None
        else:
            return node.item
```

你可能会想知道为什么这个方法会从树结构里返回元素，而不是仅仅返回一个布尔值来表示找到了这个元素。
这是因为为简单起见。
到目前为止，我们所用的素有插图都使用了数字来代表数据值。
然而，我们可以在二叉搜索树里存储任意类型的对象，对这个类型唯一的要求是对象具有可比性。
一般来说，两个对象可能`==`（相等）但它们不一定是相同的。
稍后，我们将会看到如何利用这个属性，来将我们的`BST`转换为类似于字典的对象。

为了抽象数据类型的完整，我们还应该在`BST`类里添加一个用来删除元素的方法。
从二叉搜索树中删除特定元素有点麻烦，我们有很多不同的情况需要考虑。
让我们从简单的情况开始：
如果要删除的节点是叶节点，我们可以简单地通过把它的父节点里的引用设置为`None`来将这个节点从树结构里删除。
但是，如果要删除的节点有子节点应该怎么办？
如果这个需要被删除的节点只有一个子节点的话，我们需要做的工作仍然很简单。
我们可以简单地把被删除节点的父节点里用来指向它的引用设置为它的子节点就行了。
图7.7展示了被删除节点的左子节点被提升到了它的父节点的左子节点的情况。
你可能也希望研究下其他只有单个子节点的情况（还有三个）来向自己证明，这个方式是正确的。

图7.7：从二叉搜索树中删除`4`

现在，我们继续讨论被删除的节点有两个子节点的情况，这个时候应该怎么做呢？
我们不能随便选任意一个子节点来占据被删除节点的位置，因为这可能会让另一个子节点的链接出现问题。
这种困境的一个解决方案是：
因为我们需要一个节点来维护整个树的结构，所以就简单地把这个节点留在那里就行了。
我们可以通过替换节点里的数据，而不是删除这个节点来达到这个目标。
因此，我们只需要找到一个可以被方便地删除的节点，然后把这个节点的值传输到目标节点里；
与此同时，让这颗树还能够保持树的二分查找属性就行了。

让我们来考虑图7.8里左边这颗树。
假设我们要从这棵树里删除`6`。
这颗树里还有什么值可以被放在这个位置呢？
我们可以发现，如果在这个节点里放置`5`或`7`的话，这颗二叉搜索树将会继续保持二分查找属性。
一般来说，将被删除节点里的元素替换为其直接前序节点或者直接后序节点都是正确的操作，这是因为，这些节点里的值都保证了这个节点与树里的其余节点的关系继续相同。
假设我们决定使用直接前序，那么，我们将会把这个值放入被删除的节点，然后从树里删除这个前序节点就行了。
这样的操作，将会让我们得到图7.8的右侧所展示的树。

图7.8：从二叉搜索树中删除`6`

在这个时候，你可能会担心我们需要如何去删除这个前序节点。
难道这个操作不是和删除原来那个需要被删除的节点一样，还是那么难吗？
幸运的是，事实上，并会会一样的困难。
前序节点里的值始终会是需要被删除的节点的左子树里的最大值。
很明显，要找到二叉搜索树里包含最大值的节点，我们只需要沿着树，并且始终选择右子树那个链接就行了。
当我们用完了所有的链接之后，我们就会停在最大节点上。
这也就意味着：
前序节点必然有一个空的右子树。
因此，我们总是可以通过简单地提升它的左子树来删除这个节点。

我们将再次使用在子树上的递归来实现这个算法。
我们的方法将与之前一样只包含对递归辅助函数的调用：

```Python
def delete(self, item):

    """remove item from binary search tree
    post: item is removed from the tree"""

    self.root = self._subtreeDelete(self.root, item)
```

`_subtreeDelete`方法将会是实现删除算法的核心。
它也必须返回删除元素的子树的根节点：

```Python
    def _subtreeDelete(self, root, item):
        if root is None:                # Empty tree, nothing to do
            return None
        if item < root.item:            # modify left
            root.left = self._subtreeDelete(root.left, item)
        elif item > root.item:          # modify right
            root.right = self._subtreeDelete(root.right, item)
        else:                           # delete root
            if root.left is None:       # promote right subtree
                root = root.right
            elif root.right is None:    # promote left subtree
                root = root.left
            else:
                # overwrite root with max of left subtree
                root.item, root.left = self._subtreeDelMax(root.left)
        return root
```

如果你能够把树结构理解为递归结构的话，这段代码对你来说应该不会太难理解。
这个算法里，如果需要被删除的元素是在左子树或者右子树里，我们将会递归调用`_subtreeDelete`方法来生成修改后的子树。
当（子）树的根节点是这个需要被删除的节点的时候，我们将会需要处理三种可能的情况：
提升右子树，提升左子树，或者用前序节点的元素来替换当前元素。
最后一种情况实际上可以用另一个递归方法`_subtreeDelMax`来处理。
这个方法将会查找这颗树的最大值，然后删除包含这个值的节点。
这个方法可以像下面的代码片段这样来实现：

```Python
def _subtreeDelMax(self, root):

    if root.right is None:          # root is the max
        return root.item, root.left # return max and promote left subtree
    else:
        # max is in right subtree, recursively find and delete it
        maxVal, root.right = self._subtreeDelMax(root.right)
        return maxVal, root
```

### 遍历整个二叉搜索树（`BST`）

现在，我们已经对一组元素进行了有用的抽象。
我们可以向这个集合里添加元素，查找它们，并且删除它们；
唯一还缺少的只是一些用来迭代这个集合的简单方法了。
鉴于二叉搜索树的组织方式，中序遍历会非常的实用，因为它将能够按照顺序来输出各个元素。
然而，我们的`BST`类的用户在编写自己的遍历算法的时候，并不需要知道这个数据结构的内部细节。
好在，我们有许多种可能的方法来实现这一目标。

一种方法是通过编写简单的遍历算法，来将整颗树里的元素重新组装成某种序列的形式，比如说可以组装成列表或者是队列。
我们可以通过编写递归中序遍历这个算法，来轻松地生成Python列表。
这里的代码为`BST`类提供了`asList`方法：

```Python
    def asList(self):

        """gets item in in-order traversal order
        post: returns list of items in tree in orders"""

        items = []
        self._subtreeAddItems(self.root, items)
        return items
```

```Python
    def _subtreeAddItems(self, root, itemList):
        if root is not None:
            self._subtreeAddItems(root.left, itemList)
            itemList.append(root.item)
            self._subtreeAddItems(root.right, itemList)
```

辅助函数`_subtreeAddItems`在这里执行的是树的标准的中序遍历：其中对元素的处理只是需要把这个元素附加到`itemList`就行了。
你可以比较一下这段代码和7.2节里的通用遍历算法，从而对遍历算法加深理解。
我们的`asList`方法只创建一个初始列表，然后通过调用`_subtreeAddItems`来填充这个列表。
通过添加这个方法，我们可以轻松地将`BST`对象转换为有序列表。
当然，这也就意味着我们可以通过它，来遍历集合中的所有元素。
例如，我们可以按照下面这段代码来顺序地打印出`BST`对象里的内容：

```Python
for item in myBST.asList():
    print item
```

这个遍历二叉搜索树的方案的唯一的真正的问题在于，它产生的列表和原本的集合是一样大的。
如果这个集合很大，而同时我们也只是想找到一种能够循环所有元素的方法，那么生成另一个相同大小的集合并不会是一个很好的主意。

另一个方案是：使用一个被称为*访问者模式*（*visitor pattern*）的设计模式来实现。
这种模式的思路是：
容器将会提供一个方法，这个方法能够遍历整个数据结构，并且在每个节点上都能够执行一些客户端请求的功能。
在Python里，我们可以通过一个将任意函数作为参数的方法来实现这个模式，这个方法将会把这个函数应用到树结构里的每个节点。
我们还是使用一个递归的辅助方法来实际执行整个遍历过程。

```Python
    def visit(self, f):

        """perform an in-order traversal of the tree
        post: calls f with each TreeNode item in an in-order traversal
        order"""

        self._inorderVisit(self.root, f)

    def _inorderVisit(self, root, f):
        if root is not None:
            self._inorderVisit(root.left, f)
            f(root.item)
            self._inorderVisit(root.right, f)
```

可以看到，这段代码里，`f`代表着客户端想要应用于`BST`对象里的每一个元素的任意函数。
这个函数通过`f(root.item)`这一行代码来执行。
与之前一样，这段代码只是我们的通用递归遍历算法的一个变体而已。

要使用`visit`方法，我们只需要构造一个适用于每个元素的函数就行了。
比如，假设我们仍然想按照顺序来打印出整个`BST`的内容的话，我们现在可以通过访问者模式来完成。

```Python
def prnt(item):
    print item

...
myBST.visit(prnt)
```

这里需要注意的一件事是，在调用`visit`方法的时候，`prnt`后面并没有跟上一对括号。
只有在我们真正单独调用这个函数的时候，才需要加上这对括号。
而在此刻，调用`visit`方法的时候，我们实际上并没有直接调用`prnt`函数，而是将这个函数对象本身传递给了将会实际执行调用的`visit`方法。

访问者模式为客户端代码提供了一种很好的方式来执行容器的遍历，而且还包含了一个不需要查看细节的抽象屏障。
但是编写一个恰当的函数来进行处理，在有些时候会很麻烦，并且这样的代码并不是很像Python的风格。
与我们的其他容器类一样，Python里的理想解决方案是：使用Python的生成器机制来为我们的`BST`类定义一个迭代器。
它的基本思路是：
我们将只需要编写一个通用的中序遍历，然后一次一个地`yield`树结构里的元素就行了。
在这个时候，你肯定已经非常清楚这段代码应该怎么写了。

```Python
    def __iter__(self):

        """in-order iterator for binary search tree"""

        return self._inorderGen(self.root)

    def _inorderGen(self, root):
        if root is not None:
            # yield all the items in the left subtree
            for item in self._inorderGen(root.left):
                yield item
            yield root.item
            # yield all the items from the right subtree
            for item in self._inorderGen(root.right):
                yield item
```

这段代码中唯一的与之前不一样的地方是生成器函数的递归形式。
你应该还能记得，在当你调用生成器的时候，你并没有立刻获得这个元素，而是获得了一个按需提供元素的迭代器对象。
例如，为了从左子树里实际得到元素，我们必须要遍历`self._inorderGen(root.left)`提供的迭代器，然后输出每一个元素。

这样一来，就有了一个非常方便的可以迭代我们的`BST`容器的方法了。
我们按照顺序来打印所有元素的代码不能更简单了：

```Python
for item in myBST:
    print item
```

顺便说一句，既然我们有一个`BST`类的迭代器，那么我们就不再需要有一个单独的`asList`方法了。
Python可以通过代码`list(myBST)`来使用迭代器从`BST`对象生成整个元素的列表。
能够创建包含`BST`里所有元素的列表，在为`BST`类编写单元测试的时候将会特别方便。
因为它提供了一种在断言里检查树结构的内容的简单方法。
当然，从`BST`对象获得的有序列表并不能保证树结构具有正确的形式。
为了保证树结构的正确，用另一种遍历方法（前置或后置）将会提供不少帮助。
通过检查两个不同的遍历序列来推导出二叉树的真实结构是可行的，因此如果两个遍历都是正确的话，那么你就知道树的结构与你期望的结构是相同的了。

### 二叉搜索树（`BST`）的运行时分析

在这一部分内容的介绍里，我们提到了二叉搜索树可以非常高效地维护有序集合。
我们已经向你展示了二叉搜索树是如何为我们提供有序集合的了，但是我们还没有仔细检查各个操作的运行时效率。
由于许多和树结构相关的算法都是通过递归来编写的，因此分析它们可能会看起来比较麻烦。
但是，如果我们只考虑底层结构里发生的事情的话，那么，分析起来就很容易了。

让我们先从遍历整颗树的操作开始考虑。
由于我们在每个节点上必须要完成的工作量是不变的，因此遍历的时间与树里的节点的数量是成正比的，也就是集合中的元素数量。
因此，那些操作将会是$Θ(n)$的复杂度，其中$n$是集合的大小。

对于只会检查树的一部分（例如，搜索、插入以及删除）的算法，我们的分析将会取决于树结构的形状。
所有的这些方法的最坏情况都需要走一条从树的根节点到其“底部”的路径。
很明显，这样做所需的步骤数量将和树的高度成正比。
于是，一个有趣的问题出现了，一颗二叉树有多高？
显然，这取决于树结构的确切形状。
如果我们假设，这颗树是通过按照排序的顺序来插入的一组数字的话。
这个树将会是一个链表，这是因为每个节点都被添加为前一个数字的右子节点。
对于具有$n$个元素的这个树结构来说，插入需要$n$步才能到达树的底部。

如果树结构里的数据分布得很好的话，那么我们可以估计：
对于任意给定的子树来说，都有大约一半的元素位于它的左子树，而剩下的大约一半的元素位于右子树。
我们称这样的树为“平衡”树。
相对平衡的树将具有$\log_2 n$的近似高度。
在这种情况下，必须在树中找到特定的节点的操作将会有$Θ(\lg n)$的复杂度。
好在，如果数据是按照随机的方式插入树里的话，那么当我们从根节点但向下执行的时候，对于每一个节点来说，这个元素具有同样可能性进入左子树或者右子树。
平均来说，这个结果将会是一个非常平衡的树。

在实践里，只要注意插入和删除数据的顺序，二叉搜索树通常都将会提供非常好的性能。
对于特别偏执的人来说，有一些非常有名的技术（见第13.3节）可以被用来实现二叉树，从而保证插入和删除操作之后，树结构依然平衡。

## 使用二叉搜索树（`BST`）来实现映射（选读）

上一节里，我们描述了如何让`BST`对象实现类似于有序集合的实现。
因此，我们可以插入元素、删除元素、检查元素是否存在、以及按照排序顺序来获取里面的元素。
树结构通常来说，会在类似于数据库的应用程序里被用到。
在这样的程序里，我们不仅会想要知道特定的元素是不是在集合里，而且我们还要能够去查找出具有某些特定表征的元素。
举一个简单的例子，我们可能会需要维护俱乐部会员的名单。
很明显，我们需要能够添加和删除俱乐部的成员，但我们还需要更多的东西。
比如，我们需要一种可以用来为俱乐部的特定成员提供记录，例如获取他们的电话号码。

在这一节里，我们将会了解如何扩展二叉搜索树的功能，来实现类似于Python的字典那样的通用的映射。
在我们的成员列表的示例中，我们使用了成员名称来构造的特殊“键”值，从而能够查找它的数据记录。
假设我们有一个合适的`membershipList`对象，我们可以通过下面这些代码得到一个人的电话号码：

```Python
...
info = membershipList["VanRossum, Guido"]
print info.home_phone
```

在这里，我们的`membershipList`是一个映射对象，它把成员的名称和他的具体信息的相应记录进行了映射。
我们可以使用Python字典来完成这项任务，但是字典是一种无序的映射。
而且，我们也还希望能够按照一定的顺序来高效地输出我们的（巨大的！）成员列表。

解决这个问题的一种方案是重写整个`BST`类，从而能够让它的所有方法都会有一个额外的参数，这个参数被用来获取键。
同时，这些方法还需要在执行的过程中维护这个键值对组成的树结构。
虽然，这比我们真正需要做的工作要多得多，我们还是可以通过使用现有的`BST`类来实现的。
我们可以通过一个包装这个类的包装器来实现通用映射接口，从而获得类似的效果。
这样一来，我们就可以在获得基于树结构的映射对象的优点的同时，不需要去修改`BST`类或者复制出另外一个`BST`类。
一般来说，只要有可能，我们都应该扩展现有的代码，它通常会比复制或修改现有代码的效果更好。

那么我们如何将这个`BST`类从一个集合转变为映射呢？
这里的关键是利用`BST`类里已经包含的现成的排序和查找功能。
我们的`BST`类可以被用来存储任何可以被比较的对象。
我们将会把集合里的元素存储为键值对，但诀窍是这些元素将会根据它的键进行排序。
因此，第一步是创建一个新的类来表示这些键值对元素。
我们将这个组合元素称为`KeyPair`。
同时，为了使我们的`KeyPairs`类可以被比较，我们还需要实现一些比较相关的操作。

```Python
# KeyPair.py
class KeyPair(object):

    def __init__(self, key, value=None):
        self.key = key
        self.value = value

    def __eq__(self, other):
        return self.key == other.key

    def __lt__(self, other):
        return self.key < other.key

    def __gt__(self, other):
        return self.key > other.key
```

在这里，我们只实现了六个比较运算符中的三个，这是因为`BST`类里的所有方法都只会用到这些比较运算符。
当然，为了安全起见，以防万一在将来的时候，`BST`类的代码发生了变化，我们还是应该尽量地去实现其他三个比较运算符的。
我们将会把这部分内容作为练习留给你。

有了这个`KeyPair`类之后，我们现在就可以定义一个基于`BST`类的字典映射了。
这是我们的类的构造函数。

```Python
# TreeMap.py
from BST import BST
from KeyPair import KeyPair

class TreeMap(object):

    def __init__(self, items=()):
        self.items = BST()
        for key, value in items:
            self.items.insert(KeyPair(key, value))
```

在这段代码里，我们使用了实例变量`items`来保存将会被用来储存我们的`KeyPair`元素的`BST`对象。
正如Python字典可以使用一个序列对其进行初始化一样，我们也允许`TreeMap`类的构造函数接受一个序列对。
对于这个参数，我们只需要遍历这些数据对，然后调用`BST`类的`insert`操作来填充我们的树就行了。
当然，`insert`方法将会根据键的数据来保持底层二叉搜索树的顺序，而这正是因为我们为`KeyPair`类实现了相互比较的方法。

一旦`KeyPair`对象进入到了我们的`BST`对象，我们需要能够通过它的键的数据来再次检索它。
这时，我们可以使用`BST`类的`find`操作来完成这个任务。
我们将会提供给`find`操作的参数是一个新的`KeyPair`对象，它将会等同于我们正在查找的`KeyPair`（具有相同的键）。
因此，像下面这样的一行代码，就能够解决这个问题。

```Python
        result = self.items.find(KeyPair(key))
```

要记住，`find`操作会在二叉搜索树中搜索`==`（相等）的目标元素。
这时，`KeyPair(key)`将会和`BST`中具有相同键的键值对相“匹配”，从而返回这个匹配的`KeyPair`对象。
因此，我们只需要填写键值对的键这部分数据，就能够让我们能够检索这个键的实际记录了。

为了使我们的`TreeMap`类能够像Python字典一样地工作，我们还需要实现Python里用来进行索引操作的常用的钩子函数：`__getitem__`和`__setitem__`。

```Python
def __getitem__(self, key):
    result = self.items.find(KeyPair(key))
    if result is None:
        raise KeyError()
    else:
        return result.value
```

```Python
def __setitem__(self, key, item):
    partial = KeyPair(key)
    actual = self.items.find(partial)
    if actual is None:
        # no pair yet for this key, add one
        actual = partial
        self.items.insert(actual)

    actual.value = item
```

当给定的键没有出现在字典里的时候，这些方法都会需要一些额外的工作来处理这个特殊的情况。
在这种情况下，`__getitem__`方法会抛出`KeyError`异常。
但是，当`__setitem__`得到一个新的键的时候，它需要将一个新的`KeyPair`对象插入到`BST`对象里去。
然而，由于我们已经在一开始就创建了新的`KeyPair`对象`partial`来进行初始搜索，因此把它用来设置一个新条目将会是一件简单的事情。

这些代码就足够让我们的`TreeMap`类能够启动并且运行了。
当然，这个类仍然缺少允许我们按顺序来访问所有元素的迭代器（例如，用来打印出所有成员的列表）。
我们将会把添加这些功能作为练习题留给你来完成。

## 章节总结

我们在这一章里介绍了一些用来实现树结构的基本算法和相应的数据结构。
以下是关于这些重要的亮点的快速总结：

* 树是一个非线性的容器类，它可以被用来存储分层数据或者存储有组织的线性数据，从而能够有效地去访问它们。

* 树结构通常使用链式结构来进行存储，但是它也可以被存储在数组里。

* 许多使用树结构的应用程序都使用的是二叉树，它代表着每个节点都有零个、一个或两个子节点。
  当然，也可以实现具有任意数量子节点的树。

* 二分查找属性是，对于每一个节点，它的左子树中的每一个节点的值都会小于或等于当前节点的值，它的右子树中的每一个节点的值将会大于当前节点的值。

* 二叉搜索树的搜索、插入和删除操作都支持$Θ(\lg n)$复杂度的实现，并且，这些操作还能同时保持每个节点的二分查找属性。

* 树的相关算法通常都会使用递归来编写，这是因为树本身就是一个递归数据结构。

* 三个常见的二叉树遍历顺序为：前序、中序和后序。
  二叉搜索树的中序遍历将会按照排序的顺序来输出元素。

## 练习

**判断题**

1. 树里的每一个节点最多只能有两个子节点。

2. 树节点的深度是它与树的根节点之间的节点数。

3. 树有且只有一个根节点。

4. 一个完全二叉树必然是一个满二叉树。

5. 一个满二叉树必然是一个完全二叉树。

6. 任何二叉树的中序遍历都会按照排序的顺序来输出元素。

7. 表达式树的后序遍历将会输出表达式的后缀（逆波兰）表示法。

8. 二叉搜索树的最坏情况下的搜索时间是$Θ(n)$。

9. 二叉搜索树的每个子树也是一个二叉搜索树。

10. 由于二叉树是非线性的，因此无法使用数组来实现。

**选择题**

1. 树结构是什么类型数据的自然表达？

    a) 任意互连的数据。

    b) 线性数据。

    c) 分层数据。

    d) 傻瓜数据。

2. 以下哪一项不一定适用于非空树？

    a) 它的高度至少为0

    b) 它至少有一片叶子

    c) 它至少有一个根

    d) 以上所有都适用于非空树

3. 在表达式树里，非叶节点代表：

    a) 操作数。

    b) 运算符。

    c) 括号。

    d) 符号。

4. 应该用什么顺序来遍历表达式树，从而能够计算出表达式的结果？

    a) 前序

    b) 中序

    c) 后序

    d) 位置顺序

5. 下列设计模式里的哪一个允许客户端在不知道数据结构的内部结构的情况下遍历整个数据结构？

    a) 访客模式

    b) 迭代器模式

    c) a和b

    d) 以上都不是

6. 以下哪个顺序将能够生成具有最佳搜索时间的二叉搜索树？

    a) 按照随机顺序插入元素

    b) 按照顺序插入元素

    c) 按照逆向顺序插入元素

    d) 所有的顺序都将导致相同的搜索时间

7. 递归整个树的遍历的运行时间是多少？

    a) $Θ(1)$

    b) $Θ(\lg n)$

    c) $Θ(n)$

    d) $Θ(n \lg n)$

8. 高度为`5`的二叉树里的最多可以有多少个元素？

    a) `5`

    b) `31`

    c) `32`

    d) `63`

9. 具有64个节点的树的最小高度是多少？

    a) `5`

    b) `6`

    c) `7`

    d) `32`

10. 具有64个节点的树的最大高度是多少？

    a) `6`

    b) `7`

    c) `63`

    d) `64`

**简答题**

1. 普通的二叉树的数组/列表存储方式的实现的缺点是什么？
   什么类型的二叉树将会特别适合用数组/列表来存储？

2. 对于图7.8左边的二叉搜索树（在删除节点`6`之前），列出每个遍历方法（前序、中序和后序）所访问的节点的顺序。

3. 为`BST`类写一个不变量。

4. 为`BST`类的删除操作编写先验条件与后置条件。

5. 树的排序算法通过将元素插入二叉搜索树里，然后通过中序遍历再把所有元素读回来实现的。
如果，使用树排序来对$n$个元素进行排序的话，它的渐近运行时间是多少？
讨论最坏情况和通常所期望的情况下的结果。

6. 考虑数学表达式$3 + 4 * 5$。
绘制两个不同的表达式树，并且它们的中序遍历都能够产生这个表达式。
使用7.3节里给出的评估算法来评估这两棵树。
哪一颗树对应着这个表达式的“通常”的解释？

7. 使用`TreeNode`类，来编写一个能够生成图7.5里的表达式的树结构。

8. 在这一章里，我们看到了删除二叉搜索树中的值，可以通过将其节点中的元素替换为它的中序遍历的前面那个节点的元素来进行。
我们也同时提到过，使用中序遍历的后继节点也可以完成这项任务。
假设我们不是事先就进行二选一，而是实现一个在“动态”的情况下选择这两个节点的策略。
请你提出一个合适的建议，从而能够在不同的条件下选择应该使用哪一个节点，并为执行这个判断的算法编写伪代码。

**编程练习**

1. 为`BST`类编写单元测试。

2. 在`BST`类中编写并测试递归版本的`find`函数。

3. 为`BST`类分别编写前序（`preorder`）和后序（`postorder`）遍历的生成器。
从而能够在需要生成前序遍历的列表的时候，我们可以编写这样的代码来实现`list(myBST.preorder())`。

4. 为`BST`类编写`__copy__`方法。

5. 将`__len__`操作添加到`BST`类里去。
因此，调用`len(myBST)`的时候应该能够返回`myBST`对象里的元素的数量。

6. 按照上面的最后一个简答题里你提出的建议，对`BST`的`delete`操作进行改进。

7. 基于`BST`类来实现并测试一个有序的多集合类。
多集合类是一个允许一个值出现多次的集合。
它的基本思想是树里的每一个元素都会包含值和这个值出现的次数。
你的`MultiSet`类应该包括插入、删除、计数、长度和遍历这些操作。
`count(x)`操作将会返回`x`在集合中出现的次数。
这里有一个简短的交互式实例，它可以展示出这个类的用法：

    ```Python
    >>> s = MultiSet()
    >>> for x in [3,1,4,1,5,9,2,6,5,3,5]:
    ... s.insert(x)
    >>> len(s)
    11
    >>> s.delete(5)
    >>> len(s)
    10
    >>> s.count(5)
    2
    >>> s.count(8)
    0
    >>> list(s)
    [1,1,2,3,3,4,5,5,6,9]
    ```

    注意：你可以编写一个类似于`BST`类的新类，也可以直接使用7.6节里的技术来利用现有的`BST`类。

8. 编写一个解压程序的简化版本。
下面是示例的输入文件以及用树来表示的它的代码。
第一部分的数据里，每一行都包含一个字母和它对应的代码（用空格分隔）。
它们和后面的实际编码消息之间用一个空行分开。
请注意，第一行是空格，以及它的代码；
同时，最后一行实际上是一个长行，但为了能够在页面显示，它只是被显示成了两行。
这个文件被压缩成了：

    ```
    the magic word is abracadabra
    ```

    ```
      011
    a 00
    b 1001
    c 1000
    d 1010
    e 11000
    g 11001
    h 11010
    i 1011
    m 11011
    o 11100
    r 010
    s 11101
    t 11110
    w 11111

    111101101011000011110110011001101110000111111111100010101001110111110101100100101000100000101000100101000
    ```

    从书里复制出`TreeNode.py`文件。
    在名为`PrefixCodeTree.py`的文件里创建一个叫做`PrefixCodeTree`的类。
    该类必须具有构造函数和下面两个方法：

    ```Python
    def add_code(self, letter, code):

        """add the letter and its code to the tree

        pre: letter is a string, code is a string of 0s and 1s
        corresponding to the code for the letter; code does not
        contain a prefix that has already been added
        post: the tree is updated to store the code and its
        corresponding letter"""

    def decipher_code(self, coded_msg):
        """using the tree created by the add_code calls, decipher the
        coded_msg

        pre: add_code has been called to create the tree
        coded_msg contains valid codesfor the calls to add_code
        post: returns the decode string"""
    ```

    前缀代码是指，这个代码不会被用在另一个代码的前面。
    在我们的树的表示里，叶节点对应的是字母。
    要解码消息，就需要从树的根节点开始，然后根据`0`（向左）或`1`（向右）来向下移动，直到到达叶节点，从而添加这个字母。
    在到达字母之后，下一个代码还是从树的根节点开始。
    一般来说，编码将按照二进制的格式进行存储，因此，这将会允许我们在单个字节里存储八个`0`或`1`。
    如果常用的字母的编码少于八个`0`或`1`的话，文件将会被压缩得更小。
    为了简化这个练习，我们假设文件里的内容都是纯文本。

9. 考虑在不用二叉树的情况下，可以使用Python的什么内置数据类型来通过另一种方法解决练习8的问题。
并且使用这个数据类型来解决这个问题。

10. 写一个程序来玩动物猜谜游戏。
下面是一些输出的示例，你可以通过这个实例来理解游戏的工作原理。
在其中用户的输入用粗体来显示。

    > Welcome to the Animal Game!
    >
    > You pick an animal, and I will try to guess what it is. You can    help me get better at the game by giving me more information       when I make a mistake.
    >
    > The more you play, the better I get.
    >
    > Think of an animal, and I’ll try to guess what it is.
    >
    > Is it green? **no**
    >
    > Does it purr? **n**
    >
    > Does it have black and white stripes? **y**
    >
    > Does it have hooves? **yes**
    >
    > Is your animal a(n) zebra? **yes**
    >
    > I’m soooo smart!
    >
    > Do you want to play again? **y**
    >
    > Think of an animal, and I’ll try to guess what it is.
    >
    > Is it green? **yes**
    >
    > Does it hop? **yes**
    >
    > Is your animal a(n) frog? **no**
    >
    > Rats! I didn’t get it. Please help me improve.
    >
    > What is your animal? **grasshopper**
    >
    > Please enter a yes/no question that would select between a(n) > grasshopper and a(n) frog:
    >
    > » **Does it eat leaves**
    >
    > What would the answer be for a(n) grasshopper? **yes**
    >
    > Do you want to play again? **yes**
    >
    > Think of an animal, and I’ll try to guess what it is.
    >
    > Is it green? **y**
    >
    > Does it hop? **y**
    >
    > Does it eat leaves? **y**
    >
    > Is your animal a(n) grasshopper? **yes**
    >
    > I’m soooo smart!
    >
    > Do you want to play again? **n**
    >
    > Thanks for playing!

    如你所见，这个程序演示了一种简单的机器学习的形式。
    当它无法猜测用户的动物时，它会要求用户提供更多信息，从而能够在下次可以猜出正确的动物。

    你可以使用一种被称为*决策树*（*decision tree*）的二叉树来实现这个程序。
    这个树的叶节点将会被用来代表不同的类别（在这里，代表动物），而非叶节点则包含是与否的问题。
    一轮游戏将会从树的根节点开始，然后通过询问问题，向左或者向右进行递归，向那一边取决于答案是肯定的还是否定的。
    当到达叶节点的时候，程序将会猜测叶节点上的动物。
    在猜测不正确的时候，叶节点便会会变成一个内部节点，而现在的类别将会被降级为其中的一个子节点，另一个子节点将会包含正确答案。

    你的初始的树可以只包含一个节点，这个节点里包含着你最喜欢的动物。
    随着不断地玩这个游戏，这颗树可以从这个节点开始不断的生长。
    当然，你需要一种方法能够在游戏的期间把这颗树存储到文件里去，从而能够让程序可以不断地累积经验而不是每次都需要从头开始。
    将树写入一个文件的最简单方法是使用Python的序列化功能。
    你可以看一下`pickle`模块的相关文档，来学习如何做到这一点。