#### String字符串匹配算法
@Date 2017.06.09

> [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/string/StringMatch.java)

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

> KMP匹配
* 时间复杂度O(m + n)

```
    /**
     * @param originS  长字符串
     * @param matchedS 要匹配的字符串
     * @return 匹配字符在长字符串中的位置
     */
    private static int kmpMatch(String originS, String matchedS) {
        // next 数组各值的含义：代表当前字符之前的字符串中，有多大长度的相同前缀后缀。
        // 例如如果next [j] = k，代表j 之前的字符串中有最大长度为k 的相同前缀后缀

        char[] originArray = originS.toCharArray();
        char[] matchedArray = matchedS.toCharArray();

        int originLen = originS.length();
        int matchedLen = matchedS.length();

        int[] next = getNext(matchedArray);

        int i = 0, j = 0;
        while (i < originLen && j < matchedLen) {
            if (j == -1 || originArray[i] == matchedArray[j]) {
                // 相等则继续位移比较(需要同时移位)
                i++;
                j++;
            } else {
                // j != -1，且当前字符匹配失败（originArray[i] != matchedArray[j]），则令 i 不变，j = next[j]
                // next[j]即为j所对应的next值
                // 失配时，模式串向右移动的位数为：已匹配字符数 - 失配字符的上一位字符所对应的最大长度值
                // 失配时，模式串向右移动的位数为：失配字符所在位置 - 失配字符对应的next 值
                j = next[j];
            }
        }
        // 匹配字符串完全找到
        if (j == matchedLen) {
            return i - j;
        } else {
            return -1;
        }
    }

    /**
     * 获取失配时的位移数组
     * 求解原理为:
     * 1. 匹配字符串的前缀后缀的公共元素的最大长度值的对应数组
     * 2. eg: abab : 0 0 1 2
     * 3. 最大对称长度的前缀后缀，然后整体右移一位，初值赋为-1
     * 4. eg: abab : -1 0 0 1
     * @param matchedArray 需要匹配的pattern字符串
     * @return 位移数组
     */
    private static int[] getNext(char[] matchedArray) {
        int matchedLen = matchedArray.length;
        int[] next = new int[matchedLen];
        next[0] = -1;
        int k = -1;
        int j = 0;
        while (j < matchedLen - 1) {
            // matchedArray[k]表示前缀，matchedArray[j]表示后缀
            // matchedArray[j] == matchedArray[k] 此判断是因为matchedArray[j]已经失配,用相同的matchedArray[k], 位移后去匹配一样是失配.
            // 故二者相等时,继续递归
            if (k == -1 || matchedArray[j] == matchedArray[k]) {
                ++k;
                ++j;
                next[j] = k;
            } else {
                k = next[k];
            }
        }
        return next;
    }
```