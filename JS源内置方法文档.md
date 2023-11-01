# Fun阅读JS书源内置方法列表

### 🛠网络相关方法(Http2):
    创建Http2对象;
    req = Http2.url(url： String)

    添加cookie or header
    req.header("set-cookie", ".....")
    req.header("keyName", "value")
    req.headers(Json或Json数组, 不需要JSON.stringify)

    设置请求代理
    /**
    *   host    = 代理地址  string  必须为有效的代理IP;
    *   port    = 代理端口  int     必须是有效的代理端口;
    *   type    = 代理类型 http 或 socks, 默认是http; 此参数可空;
    *   name    = 验证用户名  string, 默认为空字符串, 不验证用户名;
    *   password= 验证密码    string, 默认是空字符串, 不验证密码;
    **/
    req.proxy(host, port, type="http"|"socks", name="", password="")

    添加formdata数据  
    /**
    *   key            = 参数名称
    *   value          = 提交数据. 可以是字符串 或 ByteArray
    *   contentType    = mediaType, 不需要就填空字符串即可. 不要传入 null 或 undefined;
    **/
    req.addPart(key: String, value: String | ArrayBuffer, fileName: String = "", mediaType: String = "");


    // 例如, 请求Google搜索
    Http2.url("https://www.google.ca/search?q=%E6%88%91%E7%9A%84")
        .proxy("192.168.2.3", 1080, "http", "admin", "123456")
        .get()

    其中, 192.168.2.3 是我的电脑局域网地址, 使用ssr开启了局域网连接选项;


    开始请求
    get() or put(提交数据) or post(提交数据) or postFile(name, fileName, file: FilePathString|FileByteArray): Http2Result
    or formData()

    如果在提交post数据时, header中未指定content-type, 则App会对提交数据进行检测, 如果数据是json, 则以application/json charset=${data}.charset 进行提交；
    如果数据不是json，但是是字符串类型，则App会将其视为一段字符串数据, 并且使用默认的 application/x-www-form-urlencoded 进行提交；
    如果数据不是json，也不是字符串，那么App将其视为bytes，使用 application/octet-stream 进行提交；
    如果需要提交multipart/form-data 表单， 则提交的数据， 必须是json；

    postFile 特别说明：
        此方法有三个参数, 分别是 name, fileName, file, 
        name 是 参数名称
        fileName 是 提交文件名(不包含路径)
        file 是 文件路径字符串或文件字节数组
    
    // 提交formData()
    Http2.url("https://www.google.ca/search?q=%E6%88%91%E7%9A%84")
        .addPart("xxx", "aaaaaa")
        .formData()

    // 使用 formData() 提交文件
    Http2.url("https://www.google.ca/search?q=%E6%88%91%E7%9A%84")
        .addPart("file", 文件数据ByteArray, 提交保存文件名)
        .formData()


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


    @param url     请求网络地址;
    @paran headers 定义自定义请求头, 数据类型必须是Json或JsonArray, 如 { 'content-type': 'xxxx', ... } 或 [ 'content-type: xxxx', ... ]
    @param type    定义返回数据类型, 目前仅支持"string"与"bytes"; 默认是"string"
    @return        如果type为"bytes" 则返回ArrayBuffer(字节数组), 否则返回String; 
    GET(url: String, headers: Json|JsonArray|null, type: String = "string" | "bytes"): type

    // 本方法与GET一致, 默认返回类型是“bytes”;
    GET_BYTES(..., type='bytes')
    示例： 
        GET/GET_BYTES ("http...", { 'content-type': 'xxxx', ... } 或 [ 'content-type: xxxx', ... ])
        GET/GET_BYTES ("http...", { headers:{...}} 或 { headers:[...] })


    @param url       请求网络地址;
    @param postParam post请求数据, 支持Json、String、ArrayBuffer; 也支持在本参数内定义请求头(可以,但不建议)。  
    @paran headers   定义自定义请求头, 数据类型必须是Json或JsonArray, 如 { 'content-type': 'xxxx', ... } 或 [ 'content-type: xxxx', ... ]
    @param type      定义返回数据类型, 目前仅支持"string"与"bytes"; 默认是"string"
    @return          如果type为"bytes" 则返回ArrayBuffer(字节数组), 否则返回String; 
    POST(url: String, postParam: String|Json, headers: Json|JsonArray|null, type: String = "string" | "bytes"): type

    // 本方法与POST一致, 默认返回类型是“bytes”;
    POST_BYTES(..., type='bytes') 
    示例： 
        POST/POST_BYTES ("http...", "提交参数字符串", { 'content-type': 'xxxx', ... } 或 [ 'content-type: xxxx', ... ])
        POST/POST_BYTES ("http...", arrayBuffer, { 'content-type': 'xxxx', ... } 或 [ 'content-type: xxxx', ... ])

    另外写法, 数据和头定义在一起, 此方法不能提交ArrayBuffer数据：
        POST/POST_BYTES ("http...", { data: "", headers: {'content-type': 'xxxx', ... }})
        POST/POST_BYTES ("http...", { data: "", headers: [ 'content-type: xxxx', ... ]})


### 🛠设备方法:
    // 获得设备唯一ID, len是ID长度, 最小16, 最大32;
    getDeviceID(len=(16..32)): String

    // 获得固定UUID, 根据AndroidID运算而来;
    getUUID (): String

    // 获得随机UUID
    getRandomUUID(): String


### 🛠网页解析方法:
-[HTML2 测试示例](https://gitee.com/liliysoft/FUN_FIVE_UPDATE/blob/master/Html2Test.js)
    // 内置解析器2

    (
        本方法暂未定型, 随时可能更改, 请各位慎重使用;
    )

    // 仅 HTML2.parse() 的返回对象支持 $('选择器') 写法;
    // ResultObject 实例对象仅支持通过方法 select('选择器') 进行查询;
    let res = HTML2.parse(HTML): HTML2
    HTML2 = {
        return function(cssQuery): ResultObject 
    }    

    // 查询, 返回类型为Object, 不是数组;
    x = res('cssQuery'): ResultObject


    ResultObject = {
        select(cssQuery): ResultObject
        // select 简写; 嘎嘎嘎;
        st(cssQuery): ResultObject

        text(): String
        attr(name): String
        remove(cssQuery): undefined
        html(): String
        string(): String
        size(): Int
        isEmpty(): Bool
        toString(): String 
        toList(): Array<ResultObject>
        forEach((chihld: ResultObject) => { // your code... })
        childs(): ResultObject
    }


    // 获得字符串
    x.text(): String

    // 获得属性值
    x.attr(name): String

    // 将指定查询数据从本对象中移出;
    x.remove(cssQuery): undefined

    // 返回x中的HTML内代码
    x.string(): String

    // 返回html代码,包含父元素;
    x.html()

    // 返回已解析HTML标签数量;
    x.size(): Int

    // 检查是否为空
    x.isEmpty(): Bool

    // 将对象转为Array, 注意, Array内成员都是object, 而非字符串;
    x.toList(): Array<ResultObject>

    // 使用内置forEach, 与JS使用方法一直, 不过参数只有一个, child;
    // 例 x.forEach((chihld: ResultObject) => { // your code... })
    x.forEach()

    // 获得所有子元素
    childs(): ResultObject
    
    注注注注...注意：ResultObject 即是 x 的类型, ResultObject 对象亦可使用('cssQuery')进行查询, 如 x('cssQuery');
    


    // 内置解析器1
    let res = HTML.parse(HTML)
    x = res('jsoup选择器')
    x.text(): String
    x.attr(name): String
    x.remove('jsoup选择器')




    
### 🛠️字节数组对象相关方法(ArrayBuffer):
    // 将ArrayBuffer 转为字符串, encode 默认为utf-8;
    ArrayBuffer.toString(encode='utf-8'): String 
    
    // 将ArrayBuffer 转为hex字符串, 每个值之间用sepa分隔, 默认是一个空格;
    ArrayBuffer.toHex(sepa=' '): String

    // 将ArrayBuffer内数据进行Base64编码
    ArrayBuffer.toBase64(): String

    // 取得ArrayBuffer内数据的Md5摘要;
    ArrayBuffer.toMd5(): String
    
    // 如果ArrayBuffer内的数据是BASE64编码的数据, 可使用此方法还原数据; 
    ArrayBuffer.unBase64(): ArrayBuffer


### 🛠️字符串对象相关方法(String):
    // 将字符串进行base64编码;
    String.toBase64(encode='utf-8'): String

    // 将字符串转为hex字符串, 每个值之间用sepa分隔, 默认是一个空格; String -> ByteArray -> HexString
    String.toHex(sepa=' '): String

    // 将hex字符串转为字符串 HexString -> ByteArray -> String
    // 注意, 分隔符为空的情况下(""), 每个hex值必须是两位, 如果不够两位需要自行补零;
    // 如 ACDB8(A CD B8), A不够两位, 需要改成 0ACDB8(0A CD B8)
    hexToString(hex字符串, 分隔符=''): String

    // 将hex字符串转为ByteArray HexString -> ByteArray
    // 注意, 分隔符为空的情况下(""), 每个hex值必须是两位, 如果不够两位需要自行补零;
    // 如 ACDB8(A CD B8), A不够两位, 需要改成 0ACDB8(0A CD B8)
    hexToByteArray(data, 分隔符=''): ArrayBuffer

    // 取得字符串MD5摘要
    String.toMd5(encode='utf-8'): String

    // BASE64解码
    String.base64To(encode='utf-8'): ArrayBuffer

    // 将字符串转为字节数组
    String.toByteArray(encode='utf-8'): ArrayBuffer

    // 字符串转码;
    String.toString(oldEncode: String, newEncode: String): String	

    // 如果String是BASE64编码的字符串, 可使用此方法还原数据; 
    String.unBase64(encode='utf-8'): ArrayBuffer

    // 其他方法不再提供参数encode, 太麻烦了, 如果有需要,你可以使用toString(encode, encode) 进行转换;

    // 繁体转简体
    String.t2s(): String

    // 简体转繁体
    String.s2t(): String

    // URL编解码
    URLEncode (内容, 编码)
    URLDecode (内容, 内容编码)

### 🛠加解密相关方法:
    // 生成RSA密钥对; 返回数组, 0是公钥, 1是私钥;
    crypto.getRsaKeys (length=1024): Array<String>

    // rsa加密; isPublic 指使用的秘钥是否是公钥; 
    { // 填充模式与算法, 大小写不限
        "NoPadding"     | "NO"
        "OAEPPadding"   | "OAEP"
        "PKCS1Padding"  | "PKCS1"
        "OAEPwithSHA-1andMGF1Padding"    | "SHA1"
        "OAEPwithSHA-256andMGF1Padding"  | "SHA256"
        "OAEPwithSHA-224andMGF1Padding"  | "SHA224"
        "OAEPwithSHA-384andMGF1Padding"  | "SHA384"
        "OAEPwithSHA-512andMGF1Padding"  | "SHA512"

        "ECB"
        "NONE"
    }
    // Rsa加密明文最大长度是117字节, 若明文(数据)长度超过117字节, 则内部自动将其分段加密, 所有分段加密完毕后，进行合并；
    crypto.rsaEncode (秘钥: String|ArrayBuffer, isPublic: Boolean, 数据: Base64String|ArrayBuffer, 加密方式='ECB', 填充模式='PKCS1Padding'): ArrayBuffer
    // Rsa解密密文最大长度是128字节, 若密文(数据)长度超过128字节, 则内部自动将其分段解密，所有分段解密完毕后，进行合并；
    crypto.rsaDecode (秘钥: String|ArrayBuffer, isPublic: Boolean, 数据: Base64String|ArrayBuffer, 加密方式='ECB', 填充模式='PKCS1Padding'): ArrayBuffer
    @param isPublic 指使用的秘钥是否是公钥, true是公钥, false是私钥;
---
    { // 填充模式与算法, 大小写不限
        "ISO10126Padding" | "ISO10126"
        "NoPadding"       | "NO"    | "ZeroPadding"  | "Zero" 
        "PKCS5Padding"    | "PKCS5" | "PKCS7Padding" | "PKCS7"

        "CBC"
        "CFB"
        "CTR"
        "CTS"
        "ECB"
        "OFB"
        "GCM"
    }

    // 返回的数据会自动去除补零;
    crypto.aesEncode (秘钥: String|ArrayBuffer, 数据: Base64String|ArrayBuffer, 加密方式='CBC', 填充模式='PKCS5Padding', iv=null): ArrayBuffer
    crypto.aesDecode (秘钥: String|ArrayBuffer, 数据: Base64String|ArrayBuffer, 加密方式='CBC', 填充模式='PKCS5Padding', iv=null): ArrayBuffer
    @param 秘钥 秘钥长度必须是 16/24/32字节, 内部会自动调整密码长度, 不足16位字节,自动补零补齐16位, 超出16位不足24位,补零补齐24位, 超出24为不足32位，补零补齐32位。超出32为则抛弃32位之后的内容;

---

    { // 填充模式与算法, 大小写不限
        "ISO10126Padding" | "ISO10126"
        "NoPadding"       | "NO"    | "ZeroPadding"  | "Zero"  // 使用 NoPadding 与 ZeroPadding Fun阅读内部会自动补齐长度;
        "PKCS5Padding"    | "PKCS5" | "PKCS7Padding" | "PKCS7"

        "CBC"
        "CFB"
        "CTR"
        "CTS"
        "ECB"
        "OFB"
    }

    // 返回的数据会自动去除补零;
    crypto.desEncode (秘钥: String|ArrayBuffer, 数据: Base64String|ArrayBuffer, 加密方式='CBC', 填充模式='PKCS5Padding', iv=null): ArrayBuffer
    crypto.desDecode (秘钥: String|ArrayBuffer, 数据: Base64String|ArrayBuffer, 加密方式='CBC', 填充模式='PKCS5Padding', iv=null): ArrayBuffer

---

    { // 填充模式与算法, 大小写不限
        "ISO10126Padding" | "ISO10126"
        "NoPadding"       | "NO"    | "ZeroPadding"  | "Zero"  // 使用 NoPadding 与 ZeroPadding Fun阅读内部会自动补齐长度;
        "PKCS5Padding"    | "PKCS5" | "PKCS7Padding" | "PKCS7"

        "CBC"
        "CFB"
        "CTR"
        "CTS"
        "ECB"
        "OFB"
    }
    // 回的数据会自动去除补零;
    crypto.des3Encode (秘钥: String|ArrayBuffer, 数据: Base64String|ArrayBuffer, 加密方式='CBC', 填充模式='PKCS5Padding', iv=null): ArrayBuffer
    crypto.des3Decode (秘钥: String|ArrayBuffer, 数据: Base64String|ArrayBuffer, 加密方式='CBC', 填充模式='PKCS5Padding', iv=null): ArrayBuffer
    @param 秘钥长度必须是24位字节或以上, 如果参数提供的秘钥长度不够24字节, 则内部会自动以0补偿长度;

    // 将字符串或字节数组进行RC4加解密, 加密与解密都使用此方法;
    rc4(秘钥: String|ArrayBuffer, 秘钥: String|ArrayBuffer): ArrayBuffer
---

    RSA签名与验签
    crypto.rsaSign(私钥: String|ArrayBuffer, 数据: String|ArrayBuffer, 摘要模式: String): ArrayBuffer
    crypto.rsaSignCheck(公钥: String|ArrayBuffer, 原始数据: String|ArrayBuffer, 摘要模式: String, 签名数据: ArrayBuffer): Boolean
    { // RSA签名与验签摘要模式, 大小写不限;
        "MD2withRSA"            | "MD2"
        "MD4withRSA"            | "MD4";
        "MD5withRSA"            | "MD5"
        "MD5withRSA/ISO9796-2"  | "MD5ISO9796-2"
        "RSASSA-PSS"            | "SSA-PSS"
        "SHA1withRSA"           | "SHA1"
        "SHA1withRSA/ISO9796-2" | "SHA1ISO9796-2"
        "SHA1withRSA/PSS"       | "SHA1-PSS"
        "SHA224withRSA"         | "SHA224"
        "SHA224withRSA/PSS"     | "SHA224-PSS"
        "SHA256withRSA"         | "SHA256"
        "SHA256withRSA/PSS"     | "SHA256-PSS"
        "SHA384withRSA"         | "SHA384"
        "SHA384withRSA/PSS"     | "SHA384-PSS"
        "SHA512withRSA"         | "SHA512"
        "SHA512withRSA/PSS"     | "SHA512-PSS"
    }


### 🛠数据摘要相关方法:
    Warning Warning FBI Warning   digestHmac  与 digest 无论使用何种算法, 提供何种数据, 最后返回的一定是字节数组字符串, 即 A0DCC896... 如此格式;
    crypto.digestHmac (数据: String|ArrayBuffer, 秘钥: String|ArrayBuffer, 算法='md5|sha1|sha256|sha384|sha512'): String
    crypto.digest (数据: String|ArrayBuffer, 算法='md5|md2|sha1|sha256|sha384|sha512|crc32'): String


### 🛠杂七杂八:
    console.log(x)/ LOGE(x)/ LOGD(x)
    ENCODE(text, code=[编码, base64]): String

    // 本地数据储存操作
    localStorage.setItem (key, value)
    localStorage.getItem (key): object
    localStorage.clear ()
    localStorage.removeItem (key)
    localStorage.key (): Array<String>
    localStorage.length (): Int
    localStorage.exist (key): Boolean

    // cookie操作
    getCookie(key: String): String
    setCookie(key: String, value: String)
    removeCookie(key: String)
    hasCookie (key: String): Boolean
    getAllCookie(): [{key: , value: }, {key: , value: }, ...]
    getAllCookieKey(): [key, key, ...]

    // 对数据进行gzip解压缩;
    gzipUnPack(data: ArrayBuffer|String): ArrayBuffer
    // 对数据进行gzip压缩; 压缩完毕后返回压缩的数据
    gzipPack(data: ArrayBuffer|String): ArrayBuffer

    // 打开压缩包;密码默认是null,类型必须是字符串类型;
    // 如果data不是zip压缩字节数据,或key不正确,会导致返回null; 代码里需要做个按断;
    Zip.open(data: ArrayBuffer, key=null): ZipFile

    // 读入压缩包的指定文件; 返回字节数组;
    ZipFile.read(fileName): ArrayBuffer

    // 获得压缩包内文件数量
    ZipFile.length(): Int

    // 获得Zip压缩包内文件列表;
    ZipFile.list(): Array<String>

    // ZIP使用完毕后, 一定要close; 否则会造成内存泄漏;
    ZipFile.close(): viod
    
    // 如果你不想手动关闭zip, 请使用这个方法, 读取完毕后,自动关闭相关资源;
    Zip.open(data, key).readAndClose(fileName): ArrayBuffer


    // 时间戳格式化, 自动区分10/13位长度
    // timestamp = [ null, 'time_10' | 'time_13' | time:String|Long ]
    // timestamp参数为null,则自动生成时间戳, 为'time_10' 生成10位时间戳, 为'time_13' 生成13为时间戳, 也可以直接传入时间戳;
    // format参数为null则不进行格式化, 否则一定会按照指定格式对时间戳进行格式化;
    timestampFormat(timestamp=null, format='yyyy-MM-dd HH:mm:ss'): String

    // 获得指定长度的随机数据
    getRandomData(len=128):ArrayBuffer

    // 数组方法, 取得成员; idx >= 0, 正常,  idx < 0, 倒序取得成员;
    Array.at(idx)

    // 缓存管理
    getCacheList () //  获得本书源缓存的文件名列表 [文件名1, 文件名2....]
    clearCache() // 清空本书源缓存的所有文件;
    hasCache(name) // 检查本书源缓存目录下，是否有name文件;
    removeCache(name) // 删除name缓存文件
    writeCache(name, fileByteArray) // 保存缓存文件;
    readCache(name): ByteArray // 读入缓存文件; 
    


### Bitmap相关方法, 注意: 大写的Bitmap是类名, 小写是对象;
    // 创建方法
    Bitmap.createBytes (bytes, offset, len): Bitmap; 从bytes创建Bitmap;
    Bitmap.createSize (w, h, cfg): Bitmap; // 创建一个指定宽高的Bitmap对象; cfg为bitmap 颜色格式; cfg可忽略, 默认为RGB_565
    Bitmap.copy (bmp): Bitmap; // 复制一个bitmap;

    // 操作方法
    bitmap.fillColor(color); //bitmap填充颜色;
    bitmap.width(): Int // 宽度
    bitmap.height(): Int// 高度
    bitmap.toJavaBitmap (); // 将js bitmap对象转为java的bitmap对象;
    bitmap.toByteArray(format, quality): ByteArray; 将Bitmap转为bytes; format=Bitmap.Format, quality=压缩质量, 0..100; 默认为(PNG, 100);
    // -bitmap.drawText(str, left, top, size, color); //在Bitmap上绘制字符串;
    // -bitmap.drawRect(left, top, right, bottom, color); // 在Bitmap上绘制指定颜色的REct;
    // -bitmap.drawLine(left, top, right, bottom, lineWidth, color); // 在Bitmap上回执线条;

    bitmap.drawBitmap(bmp, srcLeft, srcTop, srcRight, srcBottom, destLeft, destTop, destRight, destBottom); // 将指定Bitmap绘制到本Bitmap上; src坐标对应bmp的选取区域, dest对应回执区域
    // src坐标和dest坐标均可为空, 如果为空, 则默认是0, src的Right和Bottom如果为空,则默认bmp的宽度与高度,  dest的Right和Bottom如果为空, 则默认为bitmap的宽度与高度;
    // drawBitmap 详解;
    如 我有一张 300*300的图像, 但是只有其中200*200的区域是有效数据，200之外的图像是花屏, 我只需要将200以内的图像, 绘制出来, 即可如此做;

    var bytes= httpLoad(...); // 假设此方法获得图像数据;
    var src= Bitmap.create(bytes, 0, bytes.size());
    var b1 = Bitmap.create(200, 200);
    b1.drawBitmap (src, 0, 0, 200, 200, 0, 0, 200, 200);

    // Bitmap.Config
    ARGB_8888 = 0, // 透明
    RGB_565   = 1, // 不透明

    // Bitmap.Format 
    PNG  = 0,
    JPEG = 1
    