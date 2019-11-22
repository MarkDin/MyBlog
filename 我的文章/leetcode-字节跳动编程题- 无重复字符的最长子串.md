## 题目

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

输入: `"abcabcbb"`
输出: 3 
解释: 因为无重复字符的最长子串是 `"abc"`，所以其
长度为 3。
示例 2:

输入: `"bbbbb"`
输出: 1
解释: 因为无重复字符的最长子串是 `"b"`，所以其长度为 1。
示例 3:

输入: `"pwwkew"`
输出: 3
解释: 因为无重复字符的最长子串是 `"wke"`，所以其长度为 3。
 **请注意，你的答案必须是子串 的长度，"pwke"是一个子序列，不是子串。**

## 算法流程

    首先生成一个标记数组,将当前出现过的字符置为1
    然后两个指针s,e 分别指向最长子串的起始和结束位置, i用来遍历整个字符串
    如果i指向的字符在标记数组中的值为0,那么e = i
    否则,标记数组中这个重复字符的值更新为当前的i; index为标记数组中与当前i指向字符重复的字符的位置,将index赋值给s; 继续扫描
```python
def length_of_longest_substring(string):
    l = len(string)
    book = {}
    answer = [0,0]
    s = e = index = 0
    for i in range(l):
        if book.get(string[i], -1) == -1:
            e = i
            book[string[i]] = i
        else:
            if e - s > answer[1] - answer[0]:
                answer[0] = s
                answer[1] = e
            # s指向出现重复的那个位置的下一个位置
            s = book[string[i]] + 1
            # 标记数组中这个重复字符的值更新为当前的i
            book[string[i]] = i

    print('最长不重复子串为: ', end='')
    print(string[answer[0]:answer[1]+1])
    print('长度为: ',answer[1]-answer[0]+1)


if __name__ == '__main__':
    s = 'abcabcbb'
    length_of_longest_substring(s)
```

