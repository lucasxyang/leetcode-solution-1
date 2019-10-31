# 1, 15, 16, 18. Sum \[Series\]

## 1. 2Sum

> Given an array of intergers, find two numbers such that they add up to a specific target number. The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2 Please note that your returned answers \(both index1 and index2\) are not zero-based.
>
> You may assume that each input would have exactly one solution.
>
> Input: numbers={2, 7, 11, 15}, target=9 Output: index1=1, index2=2

题目翻译： 这道题目的意思是给定一个数组和一个值，让求出这个数组中两个值的和等于这个给定值的坐标。输出是有要求的，1， 坐标较小的放在前面，较大的放在后面。2， 这俩坐标不能为零。

题目分析： 第一步： 我们要分析题意，其中有三个关键点：

1. 求出来的坐标值要按序排列。
2. 这两个坐标不能从零开始。
3. 这道题目假设是只有一组答案符合要求，这样降低了我们解题的难度。

根据题目我们可以得到以下信息：

1. 我们得到坐标的时候，要根据大小的顺序放入数组。
2. 因为坐标值不能为零，所以我们得到的坐标都要+1。
3. 因为有且只有一组答案符合要求，所以这大大的降低了这道题目的难度，也就是说，我们只要找到符合条件的两个数，存入结果，直接终止程序，返回答案即可。

解题思路： 这道题不是很难，是leetcode最开始的题目，要求很明确，很直接，如果我们用两个for循环，O\(n2\)的时间复杂度去求解的话，很容易计算出来，但这明显不是面试官需要的答案。brute force只有在你不知道如何优化题目的时候，将就的给出的一个解法。。那么我们能不能用O\(n\)的时间复杂度去解这道题呢？很显然是可以的，不过，天下没有掉馅饼的事情啦，既然优化了时间复杂度，我们就要牺牲空间复杂度啦。在这里用什么呢？stack？queue？vector？还是hash\_map?

对于stack和queue，除了pop外，查找的时间复杂度也是O\(n\)。明显不是我们所需要的，那么什么数据结构的查找时间复杂度小呢？很显然是 hash\_map, 查找的时间复杂度理想情况下是O\(1\)。所以我们先来考虑hash\_map，看看hash\_map怎么求解这个问题。

我们可以先把这个数组的所有元素存到hashmap中，一次循环就行， 时间复杂度为O\(n\),之后对所给数组在进行遍历，针对其中的元素我们只要用another\_number = target-numbers\[i\],之后用hashmap的find function来查找这个值，如果存在的话，在进行后续比较（详见代码），如果不存在的话，继续查找，好啦，思路已经摆在这里了，详见代码吧。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int> &numbers, int target) {
        //边角问题，我们要考虑边角问题的处理
        vector<int> ret;
        if(numbers.size() <= 1)
            return ret;
        //新建一个map<key,value> 模式的来存储numbers里面的元素和index，
        //这里的unordered_map相当于hash_map
        unordered_map<int,int> myMap;
        for(int i = 0; i < numbers.size(); ++i)
            myMap[numbers[i]] = i;
        for(int i = 0; i < numbers.size(); ++i)
        {
            int rest_val = target - numbers[i];
            if(myMap.find(rest_val)!=myMap.end())
            {
                int index = myMap[rest_val];
                if(index == i)
                    continue;     //如果是同一个数字，我们就pass，是不会取这个值的
                if(index < i)
                {
                    ret.push_back(index+1);  //这里+1是因为题目说明白了要non-zero based index
                    ret.push_back(i+1);
                    return ret;
                }
                else
                {
                    ret.push_back(i+1);
                    ret.push_back(index+1);
                    return ret;
                }
            }
        }
    }
};
```

以上解法的要注意的是记得要检查这两个坐标是不是相同的，因为我们并不需要同样的数字。

## 15. 3Sum

> Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
>
> Note: Elements in a triplet \(a,b,c\) must be in non-descending order. \(ie, a ≤ b ≤ c\) The solution set must not contain duplicate triplets.

题目翻译：

给定一个整型数组num，找出这个数组中满足这个条件的所有数字： num\[i\]+num\[j\]+num\[k\] = 0. 并且所有的答案是要和其他不同的，也就是说两个相同的答案是不被接受的。

题目的两点要求：

1. 每个答案组里面的三个数字是要从大到小排列起来的。
2. 每个答案不可以和其他的答案相同。

题目分析：

1. 每一个答案数组triplet中的元素是要求升序排列的。
2. 不能包含重复的答案数组。

解题思路：

1. 根据第一点要求： 因为要求每个答案数组中的元素是升序排列的，所以在开头我们要对数组进行排序。
2. 根据第二点要求： 因为不能包含重复的答案数组，所以我们要在代码里面做一切去掉重复的操作，对于数组，这样的操作是相同的。最开始我做leetcode的时候是把所有满足条件的答案数组存起来，之后再用map进行处理，感觉那样太麻烦了，所以这次给出的答案是不需要额外空间的。

时间复杂度分析：

对于这道题，因为是要找三个元素，所以怎样都要O\(n2\)的时间复杂度，目前我没有想出来O\(n\)时间复杂度的解法。

归根结底，其实这是two pointers的想法，定位其中两个指针，根据和的大小来移动另外一个。解题中所要注意的就是一些细节问题。好了，上代码吧。

```cpp
class Solution {
public:
//constant space version
    vector<vector<int> > threeSum(vector<int> &num) {
        vector<vector<int>> ret;
        //corner case invalid check
        if(num.size() <= 2)
            return ret;

        //first we need to sort the array because we need the non-descending order
        sort(num.begin(), num.end());

        for(int i = 0; i < num.size()-2; ++i)
        {
            int j = i+1;
            int k = num.size()-1;
            while(j < k)
            {
                vector<int> curr;   //create a tmp vector to store each triplet which satisfy the solution.
                if(num[i]+num[j]+num[k] == 0)
                {
                    curr.push_back(num[i]);
                    curr.push_back(num[j]);
                    curr.push_back(num[k]);
                    ret.push_back(curr);
                    ++j;
                    --k;
                    //this two while loop is used to skip the duplication solution
                    while(j < k&&num[j-1] == num[j])
                        ++j;
                    while(j < k&&num[k] == num[k+1])
                        --k;
                }
                else if(num[i]+num[j]+num[k] < 0)  //if the sum is less than the target value, we need to move j to forward
                    ++j;
                else
                    --k;
            }
            //this while loop also is used to skip the duplication solution
            while(i < num.size()-1&&num[i] == num[i+1])
                ++i;
        }
        return ret;
    }
};
```

根据以上代码，我们要注意的就是用于去除重复的那三个while loop。一些细节问题比如for loop中的i &lt; num.size\(\)-2;因为j和k都在i后面，所以减掉两位。当然如果写成i&lt; num.size\(\); 也是可以通过测试的，但感觉思路不是很清晰。另外一点，就是不要忘记了corner case check呀。

## 16. 3Sum Closest

> Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You man assume that each input would have exactly one solution.

题目翻译：

给定一个整形数组S和一个具体的值，要求找出在这数组中三个元素的和和这个给定的值最小。input只有一个有效答案。

题目要求：

这道题比较直接，也没有什么具体的要求。

题目分析：

1. 最短距离：两个整数的最短距离是0.这点对于这道题比较重要，别忽略。
2. 这道题和3Sum几乎同出一辙，所以方便于解题，我们还是在开头要对数组进行排序，要么没法定位指针移动。
3. 另外，这道题中用到了INT\_MAX这个值，这个值和 INT\_MIN是相对应的，在很多比较求最大值最小值的情况，经常用这两个变量。

解题思路：

这道题的解题方法和3Sum几乎相同，设定三个指针，固定两个，根据和的大小移动另外一个。属于这道题目自己的东西就是distance比较这块儿，建立一个tmp distance和min distance比较。

时间复杂度分析：

这道题目和3Sum几乎是一个思路，所以时间复杂度为O\(n2\)。

代码如下：

```cpp
class Solution {
public:

    int threeSumClosest(vector<int> &num, int target) {
        //invalid corner case check
        if(num.size() <= 2)
            return -1;

        int ret = 0;
        //first we suspect the distance between the sum and the target is the largest number in int
        int distance = INT_MAX;
        sort(num.begin(),num.end());  //sort is needed
        for(int i = 0; i < num.size()-2; ++i)
        {
            int j = i+1;
            int k = num.size()-1;
            while(j < k)
            {
                int tmp_val = num[i]+num[j]+num[k];
                int tmp_distance;
                if(tmp_val < target)
                {
                    tmp_distance = target - tmp_val;
                    if(tmp_distance < distance)
                    {
                        distance = tmp_distance;
                        ret = num[i]+num[j]+num[k];
                    }
                    ++j;
                }
                else if(tmp_val > target)
                {
                    tmp_distance = tmp_val-target;
                    if(tmp_distance < distance)
                    {
                        distance = tmp_distance;
                        ret= num[i]+num[j]+num[k];
                    }
                    --k;
                }
                else //note: in this case, the sum is 0, 0 means the shortest distance from the sum to the target
                {
                    ret = num[i]+num[j]+num[k];
                    return ret;
                }
            }
        }
        return ret;
    }
};
```

总结： 这道题的解决方法主要要注意以下几点： 1. 首先要对数组进行排序。 2. 0是两个数组间最小的距离。

## 18. 4Sum

> Given an array S of n integers, are there elements a, b, c and d in S such that a+b+c+d = target? Find all unique quadruplets in the array which gives the sume of target.
>
> Note: 1. Elements in quadruplets \(a, b, c, d\) must be in non-descending order. \(ie, a&lt;=b&lt;=c&lt;=d\) 2. The solution must not contain duplicates quadruplets.

题目翻译：

给定一个整型数字数组num和一个目标值target，求出数组中所有的组合满足条件： num\[a\]+num\[b\]+num\[c\]+num\[d\] = target.

并且要满足的条件是： 1. num\[a\] &lt;= num\[b\] &lt;= num\[c\] &lt;= num\[d\] 2. 答案中的组合没有重复的.

题目分析：

这道题和3Sum几乎同出一辙，只不过是要求四个数字的和，在时间复杂度上要比3Sum高一个数量级。对于两点要求的处理： 1. 首先要对整个数组进行排序，这样得到的答案自然是排序好的. 2. 对于重复答案的处理和3Sum是一摸一样的。

解题思路： 同3Sum.

时间复杂度分析：

这道题的解法，我选择的是空间复杂度为1， 时间复杂度为O\(n3\).对于这样的问题，如果到了KSum\(K&gt;=5\), 我觉得可以用hash\_map来牺牲空间复杂度换取好一些的时间复杂度.

代码如下：

```cpp
class Solution {
public:
    vector<vector<int> > fourSum(vector<int> &num, int target) {
        vector<vector<int>> ret;
        if(num.size() <= 3) //invalid corner case check
            return ret;
        sort(num.begin(), num.end()); //cause we need the result in quadruplets should be non-descending
        for(int i = 0; i < num.size()-3; ++i)
        {
            if(i > 0 && num[i] == num[i-1])
                continue;
            for(int j = i+1; j < num.size()-2; ++j)
            {
                if(j > i+1 && num[j] == num[j-1])
                    continue;
                int k = j+1;
                int l = num.size()-1;
                while(k < l)
                {
                    int sum = num[i]+num[j]+num[k]+num[l];
                    if(sum == target)
                    {
                        vector<int> curr; //create a temporary vector to store the each quadruplets
                        curr.push_back(num[i]);
                        curr.push_back(num[j]);
                        curr.push_back(num[k]);
                        curr.push_back(num[l]);
                        ret.push_back(curr);
                        //the two while loops are used to skip the duplication solutions
                        do{++k;}
                        while(k<l && num[k] == num[k-1]);
                        do{--l;}
                        while(k<l && num[l] == num[l+1]);
                    }
                    else if(sum < target)
                        ++k;  //we can do this operation because of we sort the array at the beginning
                    else
                        --l;
                }
            }
        }
        return ret;
    }
};
```

根据上述代码，我觉得要说明白的一点是: 1. 我用do{}while来代替了while进行重复答案的处理，为什么要这样呢？是因为如果换成了while，leetcode的test sample过不去，报的错误是超出了时间的限制，我认为如果要用while，应该是多进行了++k 还有--l的操作吧。换成了do{}while就可以通过所有的test case.

## 问题扩展

## KSum

> 根据以上的2Sum, 3Sum, 3Sum Cloest， 还有4Sum，我相信只要认真看完每道题的解法的童鞋，都会发现一定的规律，相信这时候会有人想，如果变成KSum问题，我们应该如何求解？这是个很好的想法，下面，我们来看看问题扩展.

首先，对于2Sum，我们用的解法是以空间复杂度来换取时间复杂度，那么，2Sum我们可不可以in place来解？时间复杂度又是多少？ 答案是当然可以，我们可以先sort一遍，之后再扫一变，sort的时间复杂度是O\(nlogn\)，扫一遍是O\(n\),因此，这种解法的时间复杂度是O\(nlogn\), 当然，如果对于要找index，leetcode上的题不能用这个方法，因为我们sort一遍之后，index会发生一些变化。但是我们可以用以下这个function来作为一个Helper function对于K Sum\(考虑到如果K &gt; 2, sort一遍数组的时间开销不算是主要的时间开销了\):

```cpp
void twoSum(vector<int> &numbers, int begin, int first, int second, int target, vector<vector<int>>& ret) {
       if(begin >= numbers.size()-1)
            return;
        int b = begin;
        int e = numbers.size()-1;
        while(b < e)
        {
            int rest = numbers[b]+numbers[e];
            if(rest == target)
            {
                vector<int> tmp_ret;
                tmp_ret.push_back(first);
                tmp_ret.push_back(second);
                tmp_ret.push_back(numbers[b]);
                tmp_ret.push_back(numbers[e]);
                ret.push_back(tmp_ret);
                do{b++;}
                while(b<e && numbers[b] == numbers[b-1]);
                do{e--;}
                while(b<e && numbers[e] == numbers[e+1]);
            }
            else if(rest < target)
                ++b;
            else
                --e;
        }
    }
```

给个例子，对于4Sum，我们可以调用这个function，代码如下:

```cpp
class Solution {
public:

    void twoSum(vector<int> &numbers, int begin, int first, int second, int target, vector<vector<int>>& ret) {
       if(begin >= numbers.size()-1)
            return;
        int b = begin;
        int e = numbers.size()-1;
        while(b < e)
        {
            int rest = numbers[b]+numbers[e];
            if(rest == target)
            {
                vector<int> tmp_ret;
                tmp_ret.push_back(first);
                tmp_ret.push_back(second);
                tmp_ret.push_back(numbers[b]);
                tmp_ret.push_back(numbers[e]);
                ret.push_back(tmp_ret);
                do{b++;}
                while(b<e && numbers[b] == numbers[b-1]);
                do{e--;}
                while(b<e && numbers[e] == numbers[e+1]);
            }
            else if(rest < target)
                ++b;
            else
                --e;
        }
    }
    vector<vector<int> > fourSum(vector<int> &num, int target) {
        vector<vector<int>> ret;
        if(num.size() <= 3) //invalid corner case check
            return ret;
        sort(num.begin(), num.end()); //cause we need the result in quadruplets should be non-descending
        for(int i = 0; i < num.size()-3; ++i)
        {
            if(i > 0 && num[i] == num[i-1])
                continue;
            for(int j = i+1; j < num.size()-2; ++j)
            {
                if(j > i+1 && num[j] == num[j-1])
                    continue;
                twoSum(num, j+1, num[i], num[j], target-(num[i]+num[j]), ret);
            }
        }
        return ret;
    }
};
```

以上解法可以延伸到KSum.不过是相当于对于n-2个数做嵌套循环。这么写出来使得思路清晰，以后遇到了类似问题可以解决。

