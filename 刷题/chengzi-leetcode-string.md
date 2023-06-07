---
title: 【leetcode】string的相关oj
cover: https://tuchuang-1317757279.cos.ap-chengdu.myqcloud.com/1610173947-QjrHlC-LeetCode.png
tags:
  - 刷题
  - c语言
date: 2023-06-06 17:45:18
categories: 
- 刷题
ai: ture
---

# :movie_camera:题1

给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和并同样以字符串形式返回。

你不能使用任何內建的用于处理大整数的库（比如 BigInteger）， 也不能直接将输入的字符串转换为整数形式。

 

>示例 1：
输入：num1 = "11", num2 = "123"
输出："134"
示例 2：
输入：num1 = "456", num2 = "77"
输出："533"
示例 3：
输入：num1 = "0", num2 = "0"
输出："0"

链接：https://leetcode.cn/problems/add-strings
思路：模拟简单的加法计算。设置`value1，value2，以及carry(进位)`三个变量来实现此过程。
代码：
```c++
class Solution {
public:
    string addStrings(string num1, string num2) {
        int end1=num1.size()-1;
        int end2=num2.size()-1;  //两数加法计算是从末尾开始的。注意size取值和末尾下标的大小差异！
        string news;
        int value1=0,value2=0,carry=0;
        while(end1>=0||end2>=0){ 
                 if(end1>=0)
                  value1=num1[end1--]-'0'; //一定要减'0'，才能将string的数字转为int的数字。
                  else
                   value1=0;
                 if(end2>=0)
                 value2=num2[end2--]-'0';
                 else 
                 value2=0;
                 int sum=value1+value2+carry;
                 if(sum>9){
                     carry=1;
                     sum-=10;
                 }else{
                    carry=0;
                 }
                 news+=sum+'0';
        }
        if(carry==1)  //最高位的carry可能会被遗忘！
           news+='1';
        reverse(news.begin(),news.end());
        return news;
    }
};
```
关于为何 value1=num1[end1--]-'0'中要减去一个'0':
在C++中，字符类型和数字类型之间存在一个ASCII码的`映射关系`。例如，字符'0'的ASCII码值是`48`，字符'1'的ASCII码值是`49`，以此类推。在字符串中，每个数字字符都对应着一个ASCII码值。例如，字符'0'对应着ASCII码值48，字符'1'对应着ASCII码值49，以此类推。当将一个数字字符转换为数字类型时，需要将其对应的ASCII码值减去字符'0'的ASCII码值，才能得到数字类型的值。例如，字符'1'对应的ASCII码值是49，减去字符'0'的ASCII码值48后，得到数字类型的值1。