# Z字形变换

将一个给定字符串 `s` 根据给定的行数 `numRows` ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"PAYPALISHIRING"` 行数为 `3` 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

 

**示例 1：**

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

**示例 2：**

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

**示例 3：**

```
输入：s = "A", numRows = 1
输出："A"
```

 

**提示：**

- `1 <= s.length <= 1000`
- `s` 由英文字母（小写和大写）、`','` 和 `'.'` 组成
- `1 <= numRows <= 1000`



**法一：数学计算坐标**

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows == 1)
            return s;
        int n = s.size();
        int cycle_len = 2*(numRows-1);
        string ret;
        //逐行处理
        for(int i=0;i<numRows;i++){
            for(int j=0;j<n;j+=cycle_len){
                // 竖直的两个列之间每次间隔的其实都是cycle_len，先处理这个
                if(i+j<n)
                    ret+=s[i+j];
                // 对于除了第一行和最后一行，每一次周期都会有两个数，这是第二个数，也就是斜向上的字符
                if(i!=0 && i!= numRows-1){
                    int index = j+cycle_len-i;
                    if(index<n)
                        ret += s[index];
                }
            }
        }
        return ret;
    }
};
```



**法二：模拟法**

```cpp
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows == 1)
            return s;
        vector<string> rows(numRows);
        string ret = "";
        // 模拟
        int cur_row = 0;
        bool go_down = true;
        for(char c:s){
            rows[cur_row] += c;
            // 处理更新cur_row和go_down
            if(go_down){
                cur_row++;
                if(cur_row == numRows-1)
                    go_down = false;
            }
            else{
                cur_row--;
                if(cur_row == 0)
                    go_down = true;
            }
        }
        // 逐行拼接答案
        for(string row:rows)
            ret += row;
        return ret;
    }
};
```

