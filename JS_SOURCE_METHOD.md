# Fun阅读JS书源内置方法列表

### 🛠网络相关方法(Http2):
    创建Http2对象;
    req = Http2.url(url： String)

    添加cookie or header
    req.header("set-cookie", ".....")
    req.header("keyName", "value")
    req.headers(Json或Json数组, 不需要JSON.stringify)

    设置请求代理
    req.proxy("address", port)

    开始请求
    get() or post(提交数据)

    如果在提交post数据时, header中未指定content-type, 则App会对提交数据进行检测, 如果数据是json, 则以application/json charset=${data}.charset 进行提交；
    如果数据不是json，但是是字符串类型，则App会将其视为一段字符串数据, 并且使用默认的 application/x-www-form-urlencoded 进行提交；
    如果数据不是json，也不是字符串，那么App将其视为bytes，使用 application/octet-stream 进行提交；
    如果需要提交multipart/form-data 表单， 则提交的数据， 必须是json；

