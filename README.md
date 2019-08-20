## 1. 防抖函数

将几次操作合并为一次操作进行。设置一个计时器，规定在延迟时间后触发函数，但是在延迟时间内如果再次触发，就会取消之前的计时器。如此，只有最后一次操作能触发。代码如下：

```js
function debounce(fn, wait) {
  let timer = null;
  return function() {
    let args = arguments,
      that = this;
    timer && clearTimeout(timer);
    timer = setTimeout(function() {
      fn.apply(that, args);
    }, wait);
  };
}
```

## 2. 节流函数

一定时间内只触发一次函数。并且开始触发一次，结束触发一次。代码如下

```js
function throttle(fun, delay) {
  let timer = null;
  let startTime = Date.now();
  return function() {
    let curTime = Date.now();
    let remain = delay - (curTime - startTime);
    let that = this;
    let args = arguments;
    clearTimeout(timer);
    if (remain <= 0) {
      fun.apply(that, args);
      startTime = Date.now();
    } else {
      timer = setTimeout(fun, remain);
    }
  };
}
```

## 3. 冒泡排序

```js
function bubbleSort(arr) {
  var len = arr.length;
  for (var i = 0; i < len - 1; i++) {
    for (var j = 0; j < len - 1 - i; j++) {
      // 相邻元素两两对比，元素交换，大的元素交换到后面
      if (arr[j] > arr[j + 1]) {
        var temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
  return arr;
}

//举个数组
myArr = [20, 18, 27, 19, 35];
//使用函数
bubbleSort(myArr);
```

## 4. 快速排序

```js
function quickSort(arr) {
  if (arr.length <= 1) return;

  //取数组最接近中间的数位基准，奇数与偶数取值不同，但不印象，当然，你可以选取第一个，或者最后一个数为基准，这里不作过多描述
  var pivotIndex = Math.floor(arr.length / 2);
  var pivot = arr.splice(pivotIndex, 1)[0];
  //左右区间，用于存放排序后的数
  var left = [];
  var right = [];

  console.log("基准为：" + pivot + " 时");
  for (var i = 0; i < arr.length; i++) {
    console.log("分区操作的第 " + (i + 1) + " 次循环：");
    //小于基准，放于左区间，大于基准，放于右区间
    if (arr[i] < pivot) {
      left.push(arr[i]);
      console.log("左边：" + arr[i]);
    } else {
      right.push(arr[i]);
      console.log("右边：" + arr[i]);
    }
  }
  //这里使用concat操作符，将左区间，基准，右区间拼接为一个新数组
  //然后递归1，2步骤，直至所有无序区间都 只剩下一个元素 ，递归结束
  return quickSort(left).concat([pivot], quickSort(right));

  var arr = [14, 3, 15, 7, 2, 76, 11];
  console.log(quickSort(arr));
  /*
   * 基准为7时，第一次分区得到左右两个子集[ 3, 2,]   7   [14, 15, 76, 11];
   * 以基准为2，对左边的子集[3,2]进行划分区排序,得到[2] 3。左子集排序全部结束
   * 以基准为76，对右边的子集进行划分区排序,得到[14, 15, 11] 76
   * 此时对上面的[14, 15, 11]以基准为15再进行划分区排序， [14, 11] 15
   * 此时对上面的[14, 11]以基准为11再进行划分区排序， 11  [14]
   * 所有无序区间都只剩下一个元素，递归结束
   *
   */
}
```

## 5. 选择排序

```js
var example=[8,94,15,88,55,76,21,39];
function selectSort(arr){
    var len=arr.length;
    var minIndex,temp;
    console.time('选择排序耗时')；
    for(i=0;i<len-1;i++){
        minIndex=i;
        for(j=i+1;j<len;j++){
            if(arr[j]<arr[minIndex]){
                minIndex=j;
            }
        }
    temp=arr[i];
    arr[i]=arr[minIndex];
    arr[minIndex]=temp;
    }
    console.timeEnd('选择排序耗时');
    return arr;
}
console.log(selectSort(example));
```

## 6. 数组去重

### 6.1 简单的去重方法

```js
// 最简单数组去重法
/*
 * 新建一新数组，遍历传入数组，值不在新数组就push进该新数组中
 * IE8以下不支持数组的indexOf方法
 * */
function uniq(array) {
  var temp = []; //一个新的临时数组
  for (var i = 0; i < array.length; i++) {
    if (temp.indexOf(array[i]) == -1) {
      temp.push(array[i]);
    }
  }
  return temp;
}

var aa = [1, 2, 2, 4, 9, 6, 7, 5, 2, 3, 5, 6, 5];
console.log(uniq(aa));
```

### 6.2 对象键值法去重

```js
/*
 * 速度最快， 占空间最多（空间换时间）
 *
 * 该方法执行的速度比其他任何方法都快， 就是占用的内存大一些。
 * 现思路：新建一js对象以及新数组，遍历传入数组时，判断值是否为js对象的键，
 * 不是的话给对象新增该键并放入新数组。
 * 注意点：判断是否为js对象键时，会自动对传入的键执行“toString()”，
 * 不同的键可能会被误认为一样，例如n[val]-- n[1]、n["1"]；
 * 解决上述问题还是得调用“indexOf”。*/
function uniq(array) {
  var temp = {},
    r = [],
    len = array.length,
    val,
    type;
  for (var i = 0; i < len; i++) {
    val = array[i];
    type = typeof val;
    if (!temp[val]) {
      temp[val] = [type];
      r.push(val);
    } else if (temp[val].indexOf(type) < 0) {
      temp[val].push(type);
      r.push(val);
    }
  }
  return r;
}

var aa = [1, 2, "2", 4, 9, "a", "a", 2, 3, 5, 6, 5];
console.log(uniq(aa));
```

### 6.3 排序后相邻去重

```js
/*
 * 给传入数组排序，排序后相同值相邻，
 * 然后遍历时,新数组只加入不与前一值重复的值。
 * 会打乱原来数组的顺序
 * */
function uniq(array) {
  array.sort();
  var temp = [array[0]];
  for (var i = 1; i < array.length; i++) {
    if (array[i] !== temp[temp.length - 1]) {
      temp.push(array[i]);
    }
  }
  return temp;
}

var aa = [1, 2, "2", 4, 9, "a", "a", 2, 3, 5, 6, 5];
console.log(uniq(aa));
```

### 6.4 数组下标法

```js
/*
 *
 * 还是得调用“indexOf”性能跟方法1差不多，
 * 实现思路：如果当前数组的第i项在当前数组中第一次出现的位置不是i，
 * 那么表示第i项是重复的，忽略掉。否则存入结果数组。
 * */
function uniq(array) {
  var temp = [];
  for (var i = 0; i < array.length; i++) {
    //如果当前数组的第i项在当前数组中第一次出现的位置是i，才存入数组；否则代表是重复的
    if (array.indexOf(array[i]) == i) {
      temp.push(array[i]);
    }
  }
  return temp;
}

var aa = [1, 2, "2", 4, 9, "a", "a", 2, 3, 5, 6, 5];
console.log(uniq(aa));
```

### 6.5 优化遍历数组法

```js
// 思路：获取没重复的最右一值放入新数组
/*
 * 推荐的方法
 *
 * 方法的实现代码相当酷炫，
 * 实现思路：获取没重复的最右一值放入新数组。
 * （检测到有重复值时终止当前循环同时进入顶层循环的下一轮判断）*/
function uniq(array) {
  var temp = [];
  var index = [];
  var l = array.length;
  for (var i = 0; i < l; i++) {
    for (var j = i + 1; j < l; j++) {
      if (array[i] === array[j]) {
        i++;
        j = i;
      }
    }
    temp.push(array[i]);
    index.push(i);
  }
  console.log(index);
  return temp;
}

var aa = [1, 2, 2, 3, 5, 3, 6, 5];
console.log(uniq(aa));
```

1.  array.concat(item...)

concat 方法产生一个新数组,   它包含一份 array 的浅复制(shallow copy) 并把一个或多个参数 item 附加在其后.   如果参数 item 是一个数组,   那么他的每个元素都会被分别添加.

```js
var a = ["a", "b", "c"];
var b = ["x", "y", "z"];
var c = a.concat(b, true);
console.log(a); //a还是['a', 'b', 'c']
console.log(b); //b还是['x', 'y', 'z']
console.log(c);
//输出: a, b, c, x, y, z, true
//length: 7
```

2.  array.push(item...)

push 方法把一个或多个参数 item 附加到一个数组的尾部,   和 concat 方法不同的是, 它会修改 array, 如果参数 item 是一个数组, 它会把参数数组作为单个元素整个添加到数组中, 并返回这个 array 的新长度值(length).

```js
var a = ["a", "b", "c"];
var b = ["x", "y", "z"];
var c = a.push(b, true);
console.log(a);
console.log(c);
//打印a输出: a, b, c, ['x', 'y', 'z',] true
//打印c输出: 5, 即a.length
```

3.  array.join(separator)

join 方法把一个 array 构造成一个字符串,   它先把 array 中的每一个元素构造成一个字符串, 接着用一个 separator 分隔符把他们链接在一起.   默认的 separator 是逗号”,”.   若想实现无间隔的连接,   可以使用空字符串作为 separator.

```js
var a = ["a", "b", "c"];
a.push("d");
var c = a.join("");
console.log(c);
//输出: abcd
```

提示:   将大量的字符串片段组装成一个字符串,   把这些片段放到一个数组中并用 join 方法连接起来通常比用 + 元素运算符链接这些片段要快.

4.  array.pop()

pop 和 push 方法使得数组 array 可以像堆栈(stack)一样工作.  pop 方法移除 array 中的最后一个元素并返回改元素.   如果该数组是 empty,   则返回 undefined.

```js
var first = ["a", "b", "c"];
var second = first.pop();

console.log(first); //['a', 'b']
console.log(second); // c
```

pop 可以这样实现:

```js
Array.method("pop", function() {
  return this.splice(this.length - 1, 1)[0];
});
```

5.  array.reverse()

reverse 方法反转 array 里的元素的顺序, 并返回 array 本身.

```js
var a = ["a", "b", "c"];
var b = a.reverse();
//输出a, b 都是['c', 'b', 'a']
```

6.  array.shift()

shift 方法移除数组 array 中的第一个元素并返回该元素,   如果数组 array 是空的,   它会返回 undefined,  shift 通常比 pop 慢得多.

```js
var a = ["a", "b", "c"];
var c = a.shift();
console.log(c); //输出第一个元素a
```

shift 可以这样实现:

```js
Array.method("shift", function() {
  return this.splice(0, 1)[0];
});
```

7.  array.slice(start, end)

[start, end) 闭区间 开区间

slice 方法对 array 中的一段浅复制,   首先复制 array[start],   一直复制到 array[end]为止.  end 参数是可选的, 默认值是该数组的长度 array.length.   如果两个参数中的任何一个是负数,  array.length 会和它们相加, 试图然它们成为非负数.   如果 start 大于等于 array.length,   得到的结果将会是一个新的空数组.

```js
var a = ["a", "b", "c"];
var b = a.slice(0, 1); // ['a']
var c = a.slice(1); // ['b', 'c']
var d = a.slice(1, 2); // ['b']
```

8.  array.sort(comparefn)

JavaScript 的默认比较函数吧要排序的元素都视为字符串, 它尚未足够智能到在比较这些元素之前先检测它们的类型,   所以当它比较这些数字的时候,   会把它们转化为字符串,   于是得到一个错的离谱的结果:

```js
var n = [4, 8, 15, 16, 23, 42];

n.sort;

console.log(n);

// [15, 16, 23, 4, 42, 8]
```

可以使用自己的比较函数来替换默认的比较函数,   这个自定义的比较函数接受两个参数,   并且如果这两个参数相等则返回 0, 如果第一个参数应该排在前面, 则返回一个负数;   如果第二个参数应该排在前面, 则返回一个正数:

升序:

```js
var n = [4, 8, 15, 16, 23, 42];

n.sort(function(a, b) {
  return a - b;
});

console.log(n);

//[4, 8, 15, 16, 23, 42]
```

降序:

```js
var n = [4, 8, 15, 16, 23, 42];

n.sort(function(a, b) {
  return b - a;
});

console.log(n);

//[42, 23, 16, 15, 8, 4]
```

上面的两个比较函数可以是数字正确排序, 但是不能使字符串排序.   如果我们想要给任何包含简单值得数组排序,   必须要做更多的工作:

```js
var m = ["aa", "bb", "a", 4, 8, 15, 16, 23, 42];

m.sort(function(a, b) {
  if (a === b) {
    return 0;
  }

  if (typeof a === typeof b) {
    return a < b ? -1 : 1;
  }

  return typeof a < typeof b ? -1 : 1;
});

console.log(m);

//[4, 8, 15, 16, 23, 42, "a", "aa", "bb"]
```

如果大小写不重要,   你的比较函数应该在比较值钱先将两个运算数转化为小写.

如果有一个更智能的比较函数, 我们也可以是对象数组排序,   为了让这个事情更能满足一般的情况, 需要编写一个构造比较函数的函数:

//by 函数接受一个成员名字符串作为参数;

//并返回一个可以用来对包含该成员的对象数组进行排序的比较函数

```js
var by = function(name) {
  return function(o, p) {
    var a, b;

    //typeof null === 'object'

    //o && p  用来判断参数是否为null, 必须两个都不为null; 因为null也是object

    if (typeof o === "object" && typeof p === "object" && o && p) {
      a = o[name];
      b = p[name];
      if (a === b) {
        return 0;
      }
      if (typeof a === typeof b) {
        return a < b ? -1 : 1;
      }
      return typeof a < typeof b ? -1 : 1;
    } else {
      throw {
        name: "Error",
        message: "Expected an object when sorting by " + name
      };
    }
  };
};

var s = [
  { first: "Joe", last: "Besser" },
  { first: "Moe", last: "Howard" },
  { first: "Joe", last: "DeRita" },
  { first: "Shemp", last: "Howard" },
  { first: "Larry", last: "Fine" },
  { first: "Curly", last: "Besser" }
];

s.sort(by("first"));

console.log(s);

// {first: "Curly", last: "Besser"}

// {first: "Joe", last: "Besser"}

// {first: "Joe", last: "DeRita"}

// {first: "Larry", last: "Fine"}

// {first: "Moe", last: "Howard"}

// {first: "Shemp", last: "Howard"}
```

sort 方法是不稳定的, 所以下面的调用,   不能保证产生正确的排序:

```js
s.sort(by("first")).sort(by("last"));
```

这里需要修改 by 函数,   让其可以接受第二个参数,   当主要的键值产生一个匹配的时候, 另一个 compare 方法将被调用以决出高下.

//by 函数接受一个成员名字符串和一个可选的次要比较函数作为参数

//并返回一个可以用来对包含该成员的对象数组进行排序的比较函数

//当 o[name]和 p[name]相等时, 次要比较函数被用来决出高下

```js
var by = function(name, minor) {
  return function(o, p) {
    var a, b; //typeof null === 'object' //o && p  用来判断参数是否为null, 必须两个都不为null; 因为null也是object

    if (typeof o === "object" && typeof p === "object" && o && p) {
      a = o[name];

      b = p[name];

      if (a === b) {
        return typeof minor === "function" ? minor(o, p) : 0;
      }

      if (typeof a === typeof b) {
        return a < b ? -1 : 1;
      }

      return typeof a < typeof b ? -1 : 1;
    } else {
      throw {
        name: "Error",

        message: "Expected an object when sorting by " + name
      };
    }
  };
};

var s = [
  { first: "Joe", last: "Besser" },

  { first: "Moe", last: "Howard" },

  { first: "Joe", last: "DeRita" },

  { first: "Shemp", last: "Howard" },

  { first: "Larry", last: "Fine" },

  { first: "Curly", last: "Besser" }
];

s.sort(by("last", by("first")));

console.log(s);

// {first: "Curly", last: "Besser"}

// {first: "Joe", last: "Besser"}

// {first: "Joe", last: "DeRita"}

// {first: "Larry", last: "Fine"}

// {first: "Moe", last: "Howard"}

// {first: "Shemp", last: "Howard"}
```

9.  array.splice(start, deleteCount, item...)

splice 方法从 array 中移除一个或多个元素, 并用新的 item 替换他们.   参数 start 是从数组 array 中移除元素的开始位置,   参数 deleteCount 是移除的元素个数,   如果有额外的参数,   那些 item 会插入到被移除元素的位置上.   该方法返回值为”被移除元素”的数组.

```js
var a = ["a", "b", "c"];

var r = a.splice(1, 1, "ache", "bug");

console.log(a); //['a', 'ache', 'bug', 'c']
console.log(r); //['b']
```

当数组是由多个对象组成的时候， 上面的方式就不太适用了，会出现这样的情况：

```js
let data = [
  { value: 0, name: "", selected: true },

  { value: 0, name: "" },

  { value: 0, name: "" },

  { value: 0, name: "" }
];

let insertData = [
  { value: null, name: null },

  { value: 12, name: "arrayLike" }
];

let removedData = data.splice(0, 2, insertData);

console.log("被移除的数组---->", removedData);

console.log("插入新数组后的数组---->", data);
```

打印数据：

![splice1.png](https://upload-images.jianshu.io/upload_images/15777495-58faf2ba76253d6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

没有达到预期效果，解决方案是：给第三个参数也就是要插入的数据前面加…「扩展运算符（spread）是三个点（...）」。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的「参数序列」。

```js
let removedData = data.splice(0, 2, ...insertData);
```

然后输出结果就对了：

![splice2.png](https://upload-images.jianshu.io/upload_images/15777495-28b744c016b0cdd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

splice 可以这样实现:

```js
Array.method("splice", function(start, deleteCount) {
  var max = Math.max,
    min = Math.min,
    delta,
    element,
    insertCount = max(arguments.length - 2, 0),
    k = 0,
    len = this.length,
    new_len,
    result = [],
    shift_count;

  start = start || 0;

  //如果开始的位置参数值为负数, 令其与数组的长度相加, 返回一个正数, 即为开始的index

  if (start < 0) {
    start += len;
  }

  //start要比len小, 比0大

  start = max(min(start, len), 0);

  //deleteCount须为一个number类型, 或者说如果没有传参, 则删除从start开始往后的所有元素

  deleteCount = max(
    min(typeof deleteCount === "number" ? deleteCount : len, len - start),
    0
  );

  //插入数组的长度减去删除的长度, 计算出新的数组长度; delta是插入数组和删除数组的"个数差", 可以出现负数

  delta = insertCount - deleteCount;

  new_len = len + delta;

  //如果删除元素的个数大于0, 从start+ k位置开始添加元素到result数组, 最后的返回值就是这个result数组

  // k初始值为0, 每次加一;

  while (k < deleteCount) {
    element = this[start + k];

    if (element !== undefined) {
      result[k] = element;
    }

    k += 1;
  }

  //k已经成为(deleteCount - 1)了

  //剩余数组的元素个数

  shift_count = len - start - deleteCount;

  //如果"个数差"为负数,

  if (delta < 0) {
    k = start + insertCount;

    while (shift_count) {
      this[k] = this[k - delta];

      k += 1;

      shift_count -= 1;
    }

    this.length = new_len;
  } else if (delta > 0) {
    k = 1;

    while (shift_count) {
      this[new_len - k] = this[len - k];

      k += 1;

      shift_count -= 1;
    }

    this.length = new_len;
  }

  for (k = 0; k < insertCount; k += 1) {
    this[start + k] = arguments[k + 2];
  }

  return result;
});
```

10. array.unshift(item...)

unshift 方法像 push 一样, 用于把元素添加到数组中,   但是它是把 item 插入到 array 的开始部分而不是尾部,   它的返回值是 array 的新 length.

```js
var a = ["a", "b", "c"];

var r = a.unshift("?", "@");

console.log(a); //['?', '@', 'a', 'b', 'c']
console.log(r); // 5
```

## Class 与 extends

https://blog.csdn.net/tcy83/article/details/80635758

https://blog.csdn.net/tcy83/article/details/80716205
