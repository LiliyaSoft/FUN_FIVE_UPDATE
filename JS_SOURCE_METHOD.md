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
    get() or put(提交数据) or post(提交数据): Http2Result

    如果在提交post数据时, header中未指定content-type, 则App会对提交数据进行检测, 如果数据是json, 则以application/json charset=${data}.charset 进行提交；
    如果数据不是json，但是是字符串类型，则App会将其视为一段字符串数据, 并且使用默认的 application/x-www-form-urlencoded 进行提交；
    如果数据不是json，也不是字符串，那么App将其视为bytes，使用 application/octet-stream 进行提交；
    如果需要提交multipart/form-data 表单， 则提交的数据， 必须是json；

    
    Http2Result = {     
        var code    = -1
        var body    = ByteArray(0)
        var headers = {}
        var cookies = {}

        var url      : String = ""
        var domain   : String = ""
        var error    : String = ""
        var protocol : String = "" 
    }


### 🛠设备方法:
    // 获得设备唯一ID, len是ID长度, 最小16, 最大32;
    getDeviceID(len=(16..32)): String

    // 获得固定UUID, 根据AndroidID运算而来;
    getUUID (): String

    // 获得随机UUID
    getRandomUUID(): String


### 🛠网页解析方法:
-[HTML2 测试示例](https://gitee.com/liliysoft/FUN_FIVE_UPDATE/blob/master/Html2Test.js)

    // 内置解析器2(推推推推...推荐)
    let res = HTML2.parse(HTML): 返回一个你无需关心的对象;
    
    // 查询, 返回类型为Object, 不是数组;
    x = res('cssQuery'): ResultObject

    // 获得字符串
    x.text(): String

    // 获得属性值
    x.attr(name): String

    // 将指定查询数据从本对象中移出;
    x.remove(cssQuery): undefined

    // 返回x字符串, 一般情况下等同于text()
    x.string(): String

    // 返回html代码
    x.html()

    // 返回已解析HTML标签数量;
    x.Size(): Int

    // 检查是否为空
    x.isEmpty(): Bool

    // 获得指定idx索引处成员对象;
    x.get(idx): ResultObject

    // 将对象转为Array, 注意, Array内成员都是object, 而非字符串;
    x.toList(): Array<ResultObject>

    // 使用内置forEach, 与JS使用方法一直, 不过参数只有一个, child;
    // 例 x.forEach((chihld) => { // your code... })
    x.forEach()
    
    注注注注...注意：ResultObject 即是 x 的类型, ResultObject 对象亦可使用('cssQuery')进行查询, 如 x('cssQuery');
    


    // 内置解析器1
    let res = HTML.parse(HTML)
    x = res('jsoup选择器')
    x.text(): String
    x.attr(name): String
    x.remove('jsoup选择器')