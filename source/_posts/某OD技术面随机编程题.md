---
title: 某OD技术面随机编程题
date: 2021-03-18 19:17:29
tags:
categories:
---

<!--more-->

最后大概给了不到30分钟来做这道题。
脑子没转过来，没找出它的规律。
后面面试结束后，琢磨挺久，总算还是算写出来了。
记录下。

题目大概：
编写一个函数，接收两个参数一个字符串str，一个数字numRows；
似的字符串的每个字符按照numRows来进行倒N排列输出；
例如：'PAYPALISHIRING',3

经过函数后输出如下格式:
P, ,A, ,H, ,N,
A,P,L,S,I,I,G, 
Y, ,I, ,R, , , 

```js
function transform(str, numRows) {
  let total = str.length;
  let arr = [];
  let vLen = 2 * numRows - 2; //每个V有多少字符
  let columns = Math.ceil(total / vLen) * (numRows - 1)
  for (let i = 0; i < numRows; i++) {
    let row = [];
    for (let j = 0; j < columns; j++) {
      let n = Math.floor(j / (numRows - 1)); //前面有多少V
      if ((j % (numRows - 1)) === 0) {
        let _idx = Math.floor(j / (numRows - 1)) * vLen + i;
        row[j] = str[_idx];
      } else if ((numRows - 1 - i) === (j % (numRows - 1))) {
        let _idx = n * vLen;
        // console.log(_idx + numRows + (numRows - 1 - i))
        row[j] = str[_idx + numRows + (numRows - 1 - i) - 1];
        // console.log(n, str[n + numRows - 1])
      } else {
        row[j] = ' ';
      }
    }
    arr[i] = row;
  }
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i].toString())
  }
}
transform('PAYPALISHIRING', 3)
```

