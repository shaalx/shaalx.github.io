# 树

## B- 树 [转载](https://blog.csdn.net/xdzhouxin/article/details/80015424)

B-树其实就是B树，B树是一种多路平衡搜索树

B树与二叉搜索树的最大区别在于其每个节点可以存不止一个键值，并且其子女不止两个，不过还是需要满足键值数=子女数-1。因此，对于相同数量的键值，B树比二叉搜索树要更加矮一些，特别是当M较大时，树高会更低。

![](https://img-blog.csdn.net/20180805150556313?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hkemhvdXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### B-树的插入

B树的插入首先查找插入所在的节点，若该节点未满，插入即可，若该节点以及满了，则需要将该节点分裂，并将该节点的中间的元素移动到父节点上，若父节点未满，则结束，若父节点也满了，则需要继续分裂父节点，如此不断向上，直到根节点，如果根节点也满了，则分裂根节点，从而树的高度+1。

### B-树的删除

B树的删除首先要找到删除的节点，并删除节点中的元素，如果删除的元素有左右孩子，则上移左孩子最右节点或右孩子最左节点到父节点，若没有左右孩子，则直接删除。删除后，若某节点中元素数目不符合B树要求（小于M/2-1取上整），则需要看起相邻的兄弟节点是否有多余的元素，若有，则可以向父节点借一个元素，然后将最丰满的相邻兄弟结点中上移最后或最前一个元素到父节点中（有点类似于左旋）。若其相邻兄弟节点没有多余的元素，则与其兄弟节点合并成一个节点，此时也需要将父节点中的一个元素一起合并。

## B+树

![](https://mmbiz.qpic.cn/mmbiz_png/to5t8icQhBHqk1hLPiafKCRFsqBRsZ5k0WY9YwSWtdXLdiatdibSk27BSUmSLQB459cXPB8x93EHEgGxcCCdaGDmzw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![](https://img-blog.csdn.net/20180805150951880?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hkemhvdXhpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### B+树的插入

B+树的插入与B树类似，如果节点中有多余的空间放入元素，则直接插入即可。如果节点本来就已经满了，则将其分裂为两个节点，并将其中间元素的索引放入到父节点中，在这里如果是叶子节点的话，是拷贝中间元素的索引到父节点中（因为叶子节点需要包含所有的元素），而如果是非叶子节点，则是上移节点的中间元素到父节点中。

### B+树的删除

在叶节点中删除元素，如果节点还满足B+树的要求，则okay。如果元素个数过少，并且其邻近兄弟节点有多余的元素，则从邻近兄弟节点中借一个元素，并修改父节点中的索引使其满足新的划分。如果其邻近兄弟节点也没有多余的元素，则将其和邻近兄弟节点合并，并且我们需要修改其父节点的索引以满足新的划分。并且如果父节点的索引元素太少不满足要求，则需要继续看起兄弟节点是否多余，如果没有多余则还需要与兄弟节点合并，如此不断向上，直到根节点。如果根节点中元素也被删除，则把根节点删除，并由合并来的节点作为新的根节点，树的高度减1


### B-树和B+树的区别

B+树的非叶子节点并没有指向关键字具体信息的指针，因此其内部节点相对B树更小，如果把所有同一内部节点的关键字存放在同一盘块中，盘块所能容纳的关键字数量也越多，具有更好的空间局部性，一次性读入内存的需要查找的关键字也越多，相对的IO读写次数也就降低了。

另外对于B+树来说，因为非叶子节点只是叶子节点中关键字的索引，所以任何关键字的查找都必须走一条从根节点到叶子节点的路，所有关键字查询的路径长度相同。而若经常访问的元素离根节点很近，则B树访问更迅速，因为其不一定要到叶子节点。

数据库索引采用B+树的主要原因是B树在提高了IO性能的同时并没有解决元素遍历效率低下的问题，而也正是为了解决该问题，B+树应运而生。因为叶子节点中增加了一个链指针，B+树只需要取遍历叶子节点可以实现整棵树的遍历。而且数据库中基于范围的查询是非常频繁的，B树对基于范围的查询效率太低。

## 红黑树 [参考](https://www.jianshu.com/p/e136ec79235c)

![](https://upload-images.jianshu.io/upload_images/2392382-4996bbfb4017a3b2.png?imageMogr2/auto-orient/strip|imageView2/2/w/526/format/webp)

