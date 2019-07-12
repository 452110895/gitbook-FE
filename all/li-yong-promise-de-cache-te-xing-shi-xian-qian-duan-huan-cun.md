# 利用 Promise 的 cache 特性实现前端缓存

**Cache Promise**

* 对同一个请求可能每次请求的参数不同，我需要针对不同的参数缓存不同的结果
* 我的缓存必须保证新鲜度，如果缓存时间过久需要重新请求
* 如果第一次请求失败，我不应该将失败的结果缓存下来
* 在某些情况下我想主动清除缓存

**1**：

=============================================================

function memorize\(fn, opts\) {

    opts = {

        cacheKeyFn: defaultCacheKeyFn,

        maxAge: undefined,

        cache: {},

        ...opts

    };

    const memorized = function \(...args\) {

        // ...

    };

    memorized.clear = function \(\) {

        opts.cache = {};

    };

    return memorized;

}

// 用例如下：

const request = \(userId, page\){

    return axios.get\(\`/get/something/${userId}?page=${page}\);

};

const memorizedRequest = memorize\(request, {

    // 有效期 10s

    maxAge: 10 \* 1000 

}\);

// 清除缓存

memorizedRequest.clear\(\);

=============================================================

**2**：

=============================================================

var loadPromise = null;

//定义一个变量用来保存Promise是否处于rejected状态

var loadRejected = false;

var loadData = function\(\){

  //在加载数据时，如发现loadPromise为null或promise为rejected状态，才重新加载

  if\(loadPromise === null \|\| loadRejected\) {

    //一旦准备加载数据，则重置rejected状态

    loadRejected = false;

    loadPromise = fetch\('/'\).then\(function\(data\){

      return data.statusText;

    }\).then\(undefined, function\(\){

      //如加载过程出现异常，则记录rejected状态

      loadRejected = true;

    }\);

  }

  return loadPromise;

};

var reloadData = function\(\){

  loadPromise = null;

  return loadData\(\);

};

loadData\(\).then\(function\(data\){

  console.log\(data\);

}\);

=============================================================

资料链接：

利用 **Promise** 的 **cache** 特性实现前端缓存

[https://juejin.im/entry/59e88ec7518825619a01cc77](https://juejin.im/entry/59e88ec7518825619a01cc77)

利用**promise**实现简单的前端**cache**

[https://cloud.tencent.com/developer/article/1120218](https://cloud.tencent.com/developer/article/1120218)

## 引用资料

[https://juejin.im/entry/59e88ec7518825619a01cc77](https://juejin.im/entry/59e88ec7518825619a01cc77)



