# [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

## 题目描述

>编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为 [汉明重量](http://en.wikipedia.org/wiki/Hamming_weight)).）。
>
>**提示：**
>
>- 请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
>- 在 Java 中，编译器使用 [二进制补码](https://baike.baidu.com/item/二进制补码/5295284) 记法来表示有符号整数。因此，在上面的 **示例 3** 中，输入表示有符号整数 `-3`。
>
>**示例 1：**
>
>```
>输入：n = 11 (控制台输入 00000000000000000000000000001011)
>输出：3
>解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
>```
>
>**示例 2：**
>
>```
>输入：n = 128 (控制台输入 00000000000000000000000010000000)
>输出：1
>解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
>```
>
>**示例 3：**
>
>```
>输入：n = 4294967293 (控制台输入 11111111111111111111111111111101，部分语言中 n = -3）
>输出：31
>解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
>```
>
>**提示：**
>
>- 输入必须是长度为 `32` 的 **二进制串** 。

## 解法

### 想法：位运算

对于一个数 x ，其和数字 1 按位与的结果满足如下关系：

- 如果它的二进制表达式最后一个数是1，那么 x & 1 = 1
- 如果它的二进制表达式最后一个数是0，那么 x & 1 = 0

由此我们就可以判断一个数的二进制表达式中最后一个数是不是 1 ，不过题目中要求的是所有数字为 1 的个数，那么我们可以对 x 右移 32 次，然后判断这 32 次得到结果的二进制表达式最后一个数是不是 1，累加即可得到 1 的个数之和。

时间复杂度O(1)，空间复杂度O(1) 

~~~java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int result = 0;
        for(int i=0;i<32;i++){
            if((n & 1) == 1){
                result++;
            }
            n = n >> 1;
        }
        return result;
    }
}
~~~

可以进一步对时间复杂度进行优化，上面利用数字 x 和 1 进行与运算，需要遍历 32 次。

我们可以利用 x & (x-1) 的特殊性质，不需要遍历 32 次，只需要遍历 k 次，其中 k 是数字32位二进制表达式中 1 的个数（k < =32）。

对于任意一个数 x，x & (x-1) 的结果实际上就是 x 的二进制表达式中将最后一个 1 变成 0 所得到的结果！因此我们可以不断令 `x=x&(x-1)`，不断消除 x 中的 0，知道没有为止（即 x 等于0）

![](https://cdn.jsdelivr.net/gh/SniperCoding/pictures1/20220316141023.png)

时间复杂度O(1)，空间复杂度O(1) 

~~~java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        // x = x & x-1 会将 x 二进制表达式中最后一个1变为0
        int result = 0;
        while(n!=0){
            n = n & (n-1);
            result++;
        }
        return result;
    }
}
~~~

