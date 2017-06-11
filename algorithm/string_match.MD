#### String字符串匹配算法
@Date 2017.06.09

> 暴力匹配
* 时间复杂度O(m * n)

```
    private static int forceMatch(String originS, String matchedS) {

        char[] originArray = originS.toCharArray();
        char[] matchedArray = matchedS.toCharArray();

        int originLen = originS.length();
        int matchedLen = matchedS.length();

        int i = 0, j = 0;
        while (i < originLen && j < matchedLen) {
            if (originArray[i] == matchedArray[j]) {
                // 相等则继续位移比较(需要同时移位)
                i++;
                j++;
            } else {
                // 不相等则长字符串回到原点,重新比较
                i = i - j + 1;
                j = 0;
            }
        }
        // 匹配字符串完全找到
        if (j == matchedLen) {
            return i - j;
        } else {
            return -1;
        }
    }
```