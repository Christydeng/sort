# sort
排序

// ajax

function Ajax(options) {
  // 新建一个对象，用途接受XHR对象
  var xhr = null;

  // 第一步创建XMLHttpRequest对象 || 同时兼任IE
  // 首先检测原生XHR对象是否存在，如果存在则返回它的新实例
  if (typeof XMLHttpRequest != "undefined") {
    xhr = new XMLHttpRequest();
  } else {
    // 如果这对象不存在，就抛出一个错误
    throw new Error('No XHR object available');
  }

  /**
   ** options形参解析：
   * data 发送的参数，格式为对象类型
   * url 发送请求的url，服务器地址（api）
   * async 否为异步请求，true为异步的，false为同步的
   * method http连接的方式，包括POST和GET两种方式
   */
  options = options || {};
  options.success = options.success || function () {
  };
  options.fail = options.fail || function () {
  };

  var data = options.data,
       url = options.url,
       async = options.async === undefined ? true : options.async,
       method = options.method.toUpperCase(),
       dataArr = [];

  // 遍历参数
  for (var k in data) {
    dataArr.push(k + '=' + data[k]);
  }

  // GET请求
  if (method === 'GET') {
    url = url + '?' + dataArr.join('&');
    xhr.open(method, url.replace(/\?$/g, ''), async);
    xhr.send();
  }

  // POST请求
  if (method === 'POST') {
    xhr.open(method, url, async);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    xhr.send(dataArr.join('&'));
  }

  // 响应接收完毕后将触发load事件
  xhr.onload = function () {

    /**
     * XHR对象的readyState属性
     * 0：未初始化。尚未调用open()方法。
     * 1：启动。已经调用open()方法，但尚未调用send()方法。
     * 2：发送。已经调用send()方法，但尚未接收到响应。
     * 3：接收。已经接收到部分响应数据。
     * 4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了。
     */
    if (xhr.readyState == 4) {
      // 得到响应
      if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
        // 处理成功数据
        var res;
        if (options.success && options.success instanceof Function) {
          res = xhr.responseText;
          if (typeof res === 'string') {
            res = JSON.parse(res);
            options.success.call(xhr, res);
          }
        }
      } else {
        // 处理错误数据
        if (options.fail && options.fail instanceof Function) {
          options.fail.call(xhr, res)
        }
      }

    } else {
      // 抛出检测XHR对象的readyState属性
      console.log('XHR was readyState：', xhr.readyState);
    }
  }
}

Ajax({
  url: 'https://api.github.com/users/Christydeng/starred?per_page=10&page=1',
  method: 'GET',
  async: true,
  success: function (res) {
    console.log('successful', res);
  },
  fail: function (err) {
    console.log('fail', err);
  }
})

// 冒泡排序


var ary = [9,1,2,3,4];
  var m = 0;
  for (var i = ary.length - 1; i > 0; i--) {
   var swap = false;
   for (var j = 0; j < i; j++) {
      if (ary[j] > ary[j+1]) {
        var temp = ary[j];
        ary[j] = ary[j+1];
        ary[j+1] = temp;
        swap = true;
     }
   }
   if(!swap) {
      break;
   }
   m++;
}

console.log(m);

ary = [2,1,4,6,3,8,12];
for(var i = 1; i < ary.length; i++) {
  var swap = false;
  for(j = i; j < ary.length; j++) {
    if(ary[j] > ary[j+1]) {
      var temp = ary[j];
      ary[j] = ary[j+1];
      ary[j+1] = temp;
      swap = true;
    }
  }
  if(!swap) {
    break;
  }
}
console.log(ary);

// 快速排序

function quickSort(arr){
    if(arr.length <= 1){
        return arr;
    }    
    var num = Math.floor(arr.length/2);
    var numValue = arr.splice(num,1);

    var left = [];
    var right = [];
    for(var i = 0;i<arr.length;i++){
        if(arr[i] < numValue){
            left.push(arr[i]);
        }else{
            right.push(arr[i]);
        }
    }
    return quickSort(left).concat(numValue,quickSort(right));

}
console.log(quickSort([1,45,43,4,56,7,20,1]));












