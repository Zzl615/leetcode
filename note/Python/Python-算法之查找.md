

# 查找-经典算法-Python

1. <a href="#二分查找">二分查找</a>
2. <a href="#快慢指针">快慢指针</a>
3. <a href="#经典结构">经典结构</a>：二叉搜索树、平衡二叉树、B树、红黑树
快捷键：Ctrl + Home 快速回到页面顶端查看目录，点击锚点，快速定位到算法。


**二分查找**<a name="二分查找"></a>
```python
'''二分查找
条件：只能对已经排序好的列表进行查找
需求：对搜索时间要求为O(logn)一般都是二分查找。
模板一：用于查找数组中的某个确定值，经典用法，考察一般。
模板二：有条件查找，如寻找第一个比5大的值，考察较多。
模板三：有双重条件的查找。
'''
def binarySearchOne(li, target):
    left, right = 0,len(li)-1
    while left <= right:
        mid = (left+right)//2
        if li[mid] == target:
            return mid
        #注意这里左右指针的赋值
        if li[mid] > target:
            right = mid - 1
        else:
            left = mid + 1
    #End Condition: left > right
    return -1
    
#模板一：搜索旋转数组
#排序好的数组某一部分旋转了，查找某个数。
def search(nums, target):
    start,end = 0,len(nums) - 1
    while start <= end:
        mid = (start + end) // 2
        if nums[mid] == target:
            return mid
        if nums[mid] < nums[start]: # case [7 8 0 {1} 2 3 4 5]
            if nums[mid] <= target <= nums[end] :
                start = mid + 1
            else:
                end = mid - 1
        else:# nums[mid] > nums[start] case [4 5 6 {7} 0 1 2]
            if nums[start] <= target <= nums[mid]:
                end = mid - 1
            else:
                start = mid + 1
    return -1    
    
#模板二：第一个错误的版本，某个版本出错了，之后的版本肯定也都是错的。
#思路：我们以错误的版本为目标，这样在最后判断left<right时，若left==right，则返回的值，一定是正确答案，不需要其他的判断。并且一定要计算到left==right，才可以结束。
def firstBadVersion(n):
    left,right = 1,n
    while left < right:
        mid = (left+right)//2
        if isBadVersion(mid):#返回True表示是错误的版本，False才是正确的版本
        #没用 mid - 1，是因为 mid - 1 可能不是 BadVersion，因此需要保留 mid   
            right = mid
        else:
            left = mid + 1
    #End Condition: left == right
    return right
```

**快慢指针**<a name="快慢指针"></a>
```python
'''快慢指针
寻找环入口：从起点开始，走到某处会出现一个环的入口，找出该点。
快指针比慢指针快两倍，同时出发，肯定会在某一点 C 相遇，慢指针走了 n，快指针走了 2n，因此从 C 点，慢指针再走 n 步是不是和快指针走的一样远。解释：只要在环内，就一定会相遇。
'''
def findDuplicate(self, nums):
    tortoise = hare = nums[0]
    while True:
    	tortoise = nums[tortoise]
    	hare = nums[nums[hare]]
    	if tortoise == hare:
    		break
    p1 = nums[0]
    p2 = tortoise
    while p1 != p2:
    	p1 = nums[p1]
    	p2 = nums[p2]
    return p1
```

**经典结构**<a name="经典结构"></a>
**二叉搜索树、平衡二叉树、B树、红黑树**
1、二叉搜索树：左边的值小于根节点，右边的值大于根节点。
  二叉搜索树缺点：会出现全部节点在一端的情况，升级为平衡二叉树[AVL]。
2、平衡二叉树[AVL]：根节点左右两边深度差不超过1。
  平衡二叉树[AVL]缺点：对于插入删除频繁的操作，需要多次旋转，升级为红黑树，平衡二叉树性  质的改进。
  平衡二叉树[AVL]缺点：每个节点只存放一个元素，最多只有两个子节点，查找需要多次磁盘IO（不同层数据存放在不同页），升级为B树。

3、B树也叫B-树，一种多路平衡树。查找不稳定，遍历比较麻烦，分支多层数少。B树每一层存放了更多的节点，由AVL树的“瘦高”变成了“矮胖”。可以相对减少磁盘IO的次数。一般用于数据库中做索引，MongoDB的索引就是用 B 树实现的。[B树参考博客](https://www.cnblogs.com/nullzx/p/8729425.html)
  B树缺点：查找不稳定，遍历比较麻烦，升级为B+树。

4、B+树：每个非叶子结点存放的元素只用于索引作用，所有数据保存在叶子结点。一般来说都会进行一个优化，就是将所有的叶子节点用指针串联起来，遍历叶子节  点就能获取全部数据，这样就能进行区间访问了，每个非叶子结点存放的元素只用于索引作用，所有数据保存在叶子结点。
[B+树参考](https://blog.csdn.net/wanderlustLee/article/details/81297253)

5、红黑树：[参考博客](https://blog.csdn.net/net_wolf_007/article/details/79706498)
由来：对平衡二叉树的限制条件，通过对任何一条从根到叶子的简单路径上各个节点的颜色进行约束，确保没有一条路径会比其他路径长2倍，因而是近似平衡的。所以相对于严格要求平衡的AVL树来说，它的旋转保持平衡次数较少。用于搜索时，插入删除次数多的情况下我们就用红黑树来取代AVL。
规则：
特征一：节点要么是红色，要么是黑色（红黑树名字由来）。
特征二：根节点是黑色的
特征三：每个叶节点(nil或空节点)是黑色的。
特征四：每个红色节点的两个子节点都是黑色的（相连的两个节点不能都是红色的）。
特征五：从任一个节点到其每个叶子节点的所有路径都是包含相同数量的黑色节点。


```python
'''二叉搜索树
如果满足：(L.val-root.val)*(R.val-root.val) <= 0，那么LR肯定在根节点两侧。
1、二叉搜索树查找某个值
2、普通树查找某个值
3、中序遍历的方式将二叉搜索树转为数组
4、判断是否为平衡二叉树
'''
def SearchOne(root, val):
    while root:
    	if root.val == val:
    		return root
    	elif root.val > val:
    		root = root.left
    	else:
    		root = root.right
    return None

#寻找某个值，用迭代的方式遍历所有节点，适用于所有树
def SearchTwo(root, val):
	stack = [root]
	while stack:
		node = stack.pop(0)
		if node == None:
			continue
		if node.val == val:
			return node
		stack.append(node.left)
		stack.append(node.right)
	return None

#中序遍历的方式将二叉搜索树转为数组
def traverse(root):
	if root == None:
		return []
	return traverse(root.left) + [root.val] + traverse(root.right)

#判断是否为平衡二叉树
def isBalanced(root):
	def dfsBalanced(node):
	    if not node: return 0 #返回数据0
	    left = self.dfsBalanced(node.left)
	    right = self.dfsBalanced(node.right)
	    if left == -1 or right == -1 or abs(left-right)>1:
	        return -1
	    return max(left, right)+1#注意返回条件的选择
    return self.dfsBalanced(root) >= 0
```

<a name="简单选择排序"></a>
```python
'''简单选择排序
选择理由：不稳定，很无序，与原位置偏离较远。
基本思想:后面找到最小的值后再来交换，比冒泡排序的优点是不用交换那么多次[可能也是唯一的优点吧]。
时间复杂度：无论好差平均都是O(n2)
空间复杂度：O(1)
'''
def SelectSort(li):
    for i in range(len(li)-1):#定义趟数n-1
        minIndex = i
        for j in range(i+1,len(li)):#定义每趟排序的比较次数
            if li[minindex] > li[j]:
                minindex = j
        if minindex != i:
            li[i],li[minindex] = li[minindex],li[i]
```

**前面三种最差都是O(n2)，经过后人的不断努力，研究，速度才得以提升。排序要加快的基本原则之一，是让后一次的排序进行时，尽量利用前一次排序后的结果，以加快排序的速度。以下介绍几种进阶排序：希尔排序、堆排序、归并排序、快速排序、计数排序**

<a name="希尔排序"></a>

```python
'''希尔排序-缩小增量排序
选择理由：不稳定，适合基本无序的序列，内部使用了简单插入排序。
基本思想:我们比较的元素可以再远一点，即所谓的增量，等距的一些元素先排序，再逐渐过渡到整个数组，最后的增量肯定是 1，检查还有哪些没有排好序。
时间复杂度：最差O(n2)，最好平均不确定(增量的选择很关键，有很多研究)
空间复杂度：O(1)
'''
import random
def ShellSort(li):
    def shellInsert(li, d):#使用的是直接插入排序
        n = len(li)
        for front in range(d, n):
            rear = front - d
            temp = li[front]
            while rear >= 0 and li[rear] > temp:
                li[rear+d] = li[rear]
                rear -= d
            if li[front] != temp:
                li[rear+d] = temp
    n = len(li)
    if n <= 1:
        return li
    d = n // 2
    while d >= 1:
        shellInsert(li, d)
        d //= 2
    return li
#测试
li = [x for x in range(10)]
random.shuffle(li)
print(li)
print(ShellSort(li))
```

<a name="堆排序"></a>

```python
'''堆排序
选择理由：简单选择排序没有很好利用第一次比较的结果，因此改进为堆排序,不稳定。
基本思想:建堆：从最后一个非叶子节点开始调整到根节点；
大顶堆：将最大值移动到根节点，父节点：一定比所有的子节点都大
小顶堆：将最小值移动到根节点，父节点：一定比所有的子节点都小
排序：从根节点取出数放在最后面n-1次。这里并不直接使用数，而是用数组模拟树结构，数组到树的转换，下标从 0 开始，节点若存在父节点则下标为 n//2，若有子节点则左子节点下标为 2*n+1，右子节点下标为 2*n+2.

时间复杂度：最好-最差-平均都是：O(nlogn)这就很厉害了！
空间复杂度：O(1)

堆排序经典应用：
1. 三角形的最大周长：给定一个长度数组，返回三条构成三角形的最大周长。
解题思路：不一定要将数组全部排序，构造大顶堆，边排序边判断两边之和大于第三边即可。
'''
def heapadjust(li, root, end):
    leaf = root*2+1
    tmp = li[root]
    while leaf <= end:
        if leaf < end and li[leaf+1] > li[leaf]:#比较两个子节点
            leaf += 1
        #迭代后不是和父节点比较，因为我没有交换，和保存父节点值的tmp比较
        if li[leaf] > tmp:
            li[root] = li[leaf]# 将父节点替换成新的子节点的值，这里就完成了转换。
            root = leaf# 赋坐标，为下一步迭代做好准备，子节点变成父节点
            leaf = root*2+1#寻找子节点的子节点
        else:
            break
    li[root] = tmp# 将转换的值赋给最终移动到的地方。

def heapsort(li):
    n = len(li)
    # 创建堆，和下面排序不同的是，我们转换的范围永远是整个数组n-1
    for not_leaf in range(n//2-1, -1, -1):#下标从零开始。
        heapadjust(li, not_leaf, n-1)

     # 挨个出数，创建堆已经完成了第一次排序
    for end in range(n-1, -1, -1):
        li[end],li[0] = li[0],li[end] # 将最后一个值与父节点交互位置
        heapadjust(li, 0, end-1)
```

<a name="归并排序"></a>

```python
'''归并排序-分治递归的思想,代码为二路归并
选择理由：稳定，一般用于总体无序，但各子项相对有序。

基本思想:将列表分成几部分，两两合并。
时间复杂度：最好-最差-平均都是：O(nlogn)这就很厉害了！
空间复杂度：O(n)

归并排序经典应用：
1. 排序链表：
解题思路:递归归并
经典考察三个知识点：归并排序，找中间节点，合并链表
'''
def MergeSort(li):
    #合并左右子序列函数
    def merge(li, left, mid, right):
        temp = [] #中间数组
        i = left#左段子序列起始
        j = mid + 1#右段子序列起始
        while i<= mid and j <= right:
            if li[i] <= li[j]:#要加等号，否则不稳定。
                temp.append(li[i])
                i += 1
            else:
                temp.append(li[j])
                j += 1
        while i <= mid:
            temp.append(li[i])
            i += 1
        while j <= right:
            temp.append(li[j])
            j += 1
        # !注意这里，不能直接li=temp,他俩大小都不一定一样
        for i in range(left, right+1):
            li[i] = temp[i-left]
    #递归调用归并排序
    def msort(li, left, right):
        if left >= right:
            return
        mid = (left+right)//2
        mSort(li, left, mid)
        mSort(li, mid+1, right)
        merge(li, left, mid, right)
    n = len(li)
    if n <= 1:
        return li
    mSort(li, 0, n-1)
    return li
```

<a name="快速排序"></a>

```python
'''快速排序-20世纪十大算法之一，别问我其他九大算法。
选择理由：不稳定，但是后序研究丰富，很多优化方案。进行优化后，该排序算法依旧很强！
优化1：三数取中，即不再将最左边的值当做中间枢纽。而是取左中右三个值。
优化2：对于大数据是效果较好，但是如果数据本身就很小，反而更费事了，因此采用分区大小调整，当数据集
较小时，不必继续递归调用快速排序算法，而改为调用其他的对于小规模数据集处理能力较强的排序算法来完成。
一般使用插入排序，因为当区间较小时，已经几乎完成排序，有着接近线性的复杂度
优化4：数据较小更换排序算法

基本思想:选一个数作为中间枢纽，进行列表的划分，递归。
时间复杂度：最差O(n2)，最好O(nlogn)，平均O(nlogn) -由分区的好坏决定。
空间复杂度：O(n)，主要为递归造成的空间栈的使用
'''
def QuickSort(li):
    def partition(li, left, right):
        key = left#优化1：三数取中，随机选择一个等都会达到O(nlogn)
        while left < right:
            #先从后面找，忽略相等的元素。最后相遇一定是在小元素。
            while left < right and li[right] >= li[key]:
                right -= 1
            while left < right and li[left] <= li[key]:
                left += 1
            li[left],li[right] = li[right],li[left]
              #优化2：直接进行交换，最后再插入中间枢纽:。
        li[key],li[left] = li[left],li[key]
        return left

    def quicksort(li, left, right):#递归调用
        if left >= right:
            return
        mid = partition(li, left, right)
        quicksort(li, left, mid-1)
        #优化3：减少递归栈空间的使用，使用迭代
		#while循环加上left = mid + 1
        quicksort(li, mid+1, right)
    #主函数
    n = len(li)
    if n <= 1:
        return li
    quicksort(li, 0, n-1)
    return li


```

<a name="计数排序"></a>

```python
'''计数排序-牺牲空间复杂度来使时间复杂度达到线性增长，唯一不使用比较的排序算法。
时间复杂度为 O(n) 有点意思哦！
空间复杂度为 O(n)

对于小规模排序计数排序的时间复杂度和空间复杂度都是效率较高的，但是计数排序对输入有限制，并不是所有情况下都能使用这种排序算法。计数排序的一个大前提就是序列中的每个数字都必须为0到k的整数。

计数排序经典应用：
1. 最大间距
应用：给500万考生成绩排序，超级快，适用于元素集合不大的情况，分数最多是0-1000。
应用：在一个文件中有 10G 个整数，乱序排列，要求找出中位数，内存限制为 2G，只写出思路即可。
'''
def CountAlgorithm(org_li, max_num):
    temp_li = [0]*(max_num+1)
    sorted_li = [0]*len(org_li)
    #统计原始列表中不同数字的个数，以数字为下标，在新列表中统计
    for i in range(len(org_li)):
        temp_li[org_li[i]] += 1
    print(temp_li)

    #统计小于当前坐标的数字有多少个,迭代方式   
    for i in range(1, max_num+1):
        temp_li[i] += temp_li[i-1] 
    print(temp_li)

    #找到某个数的个数减一，就是他在新列表的坐标。
    for x in org_li[::-1]:#如果要让它是稳定的，需要逆序输出
        temp_li[x] -= 1
        sorted_li[temp_li[x]] = x
    return sorted_li
org_li=[2,5,3,0,2,3,0,3]
max_num = 5
print(CountAlgorithm(org_li,max_num))
```

  <a name="桶排序"></a>

```python
'''桶排序：工作的原理是将数组分到有限数量的桶子里。每个桶子再个别排序
（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。
桶排序是鸽巢排序的一种归纳结果。当要被排序的数组内的数值是均匀分配的时候，
桶排序使用线性时间（Θ（n））。但桶排序并不是比较排序，他不受到 O(n log n)下限的影响。
'''
def bucketSort(nums):
#选择一个最大的数
  max_num = max(nums)
#创建一个元素全是0的列表, 当做桶
  bucket = [0]*(max_num+1)
#把所有元素放入桶中, 即把对应元素个数加一
  for i in nums:
    bucket[i] += 1
#存储排序好的元素
  sort_nums = []
#取出桶中的元素
  for j in range(len(bucket)):
    if bucket[j] != 0:
      for y in range(bucket[j]):
        sort_nums.append(j)
  return sort_nums
nums = [5,6,3,2,1,65,2,0,8,0]
print(bucketSort(nums)) 
```



**下面是一些经典排序的算法**

  <a name="乱数排序"></a>

```python
'''洗扑克牌-乱数排序
思路：如何有效打乱一个数组：遍历数组的位置，然后产生随机数，将随机数位置与当前位置交换即可。
输入：poker = [1,...,52]
'''
import random
def washPoker(poker):
    for i in range(52):
        j = random.randint(0,51)
        poker[i],poker[j] = poker[j],poker[i]
    return poker
```

  <a name="三色旗"></a>

```python
'''三色旗排序法
题目概述：将蓝白红三种旗子，按照顺序排好，要求移动次数最少，一次只能调换两个旗子。
解题思路：设置三个指标，blue = white = 0，red = len-1,然后从左往右判断，如果旗子颜色是，白色：w++，蓝色： swap(b++,w++)，红色： r 向左移动到非红色，再 swap(w,r)
'''
def AlgorithmGossip(flags):
    b,w,r = 0,0,len(flags)-1
    while w <= r:
        if flags[w] == 'w':
            w += 1
        elif flags[w] == 'b':
            flags[b],flags[w]=flags[w],flags[b]
            b += 1
            w += 1
        else:
            while w < r and flags[r] == 'r':
                r -= 1
            flags[w],flags[r] = flags[r],flags[w]
            r -= 1
    return flags
```

  <a name="摆动排序"></a>

```python
'''摆动排序
题目概述：将数组排序成a <= b >= c <= d >= e......
解题思路1: 复杂度O(nlogn)，先排序，再调换二三，四五，...等的位置
half = (len(nums)+1)//2
nums.sort()
nums[::2],nums[1::2] = nums[:half][::-1],nums[half:][::-1]

解题思路2: 复杂度O(n)，分析可知，当i为奇数时，nums[i] >= nums[i - 1]
当i为偶数时，nums[i] <= nums[i - 1]
'''
def One(nums):
    nums.sort()
    if len(nums) <= 2:
        return 
    for i in range(2, len(nums), 2):
        nums[i],nums[i-1] = nums[i-1],nums[i]

def Two(nums):#不能有相同的值，否则会出错
    if len(nums) <= 1:
        return
    for i in range(1, len(nums)):
        if (i%2==1 and nums[i] < nums[i-1]) || (i%2==0 and nums[i] > nums[i-1]):
            nums[i],nums[i-1] = nums[i-1],numss[i]
```

  <a name="煎饼排序"></a>

```python
'''煎饼排序
题目概述：给定数组 A=[1,...,n] 不重复打乱顺序的连续值，我们可以对其进行煎饼翻转：我们选择一些正整数 k <= A.length，然后反转 A 的前 k 个元素的顺序。我们要执行零次或多次煎饼翻转（按顺序一次接一次地进行）以完成对数组 A 的排序。返回能使 A 排序的煎饼翻转操作所对应的 k 值序列。任何将数组排序且翻转次数在 10 * A.length 范围内的有效答案都将被判断为正确。来源：力扣（LeetCode）
解题思路:倒序排，将 A 中的元素从大到小依次反转到数组的尾部。

'''
def nreverse(A, p):#将前 p 个元素反转。
    A[:p] = reversed(A[:p])
def pancakeSort(A):
    tail,res = len(A),[]
    while tail > 1:
        p = A.index(tail)#当前乱序的最大值的坐标
        if p != 0:
            num  = p + 1
            self.nreverse(A, num)
            res.append(num)
        self.nreverse(A, tail)
        res.append(tail)
        tail -= 1
    return res
```

  <a name="距离相等的条形码"></a>

```python
'''距离相等的条形码
题目概述：数组中相同的数字不能相连。[1,1,1,2,2,2]
解题思路:按照数量多少先进行排序，然后按奇偶填充。
'''
def rearrangeBarcodes(barcodes):
    counter = dict(collections.Counter(barcodes))
    #按出现次数统计元素
    sortedCounter = sorted( counter, key=lambda k: 0 - counter[k])
    barcodes = []
    #重新排序
    for i in sortedCounter:
        barcodes += [i] * counter[i]
        arrangedBarcodes = [None for _ in range(len(barcodes))]
#间隔插入
arrangedBarcodes[::2] = barcodes[:len(arrangedBarcodes[::2])]
arrangedBarcodes[1::2] = barcodes[len(arrangedBarcodes[::2]):]

return arrangedBarcodes
```

  <a name="距离顺序排列矩阵单元格"></a>

```python
'''距离顺序排列矩阵单元格
题目概述：给定矩阵中的一个坐标，按曼哈顿距离排列矩阵中其余的坐标。
曼哈顿距离：|r1 - r2| + |c1 - c2|
解题思路：利用空间换时间的想法，构造距离空间。
'''
def allCellsDistOrder(R, C, r0, c0):
    ans = []
    dic = {}
    for i in range(R):
        for j in range(C):
            diff = abs(i-r0)+abs(j-c0)
            print(diff)
            if diff not in dic:
                dic[diff] = [[i,j]]
            else:
                dic[diff].append([i,j])
                for i in range(R+C):
    if i in dic.keys():
        ans.extend(dic[i])
return ans
```





