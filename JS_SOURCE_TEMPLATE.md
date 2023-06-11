编写JS源时,请参考本模板;

1. 搜索方法
/**
* @param  key   搜索内容
*/
function search (key: String) {  }
@RETURN:
@其中, 名称前标有*号的, 是必须存在的;
[
    {
        * name: ,   // 书籍名称
        * author: , // 书籍作者
        * detail: , // 书籍URL或地址
        cover: ,    // 书籍封面
        summary: ,  // 书籍简介
        category: , // 书籍类型
        status: ,   // 书籍状态, 统一为 连载, 完结
        lastChapter: ,  // 最后章节名
        words: ,        // 书籍字数
        update: ,       // 更新日期时间戳或字符串
        tags: ,         // 书籍tags
        score:,         // 书籍得分、评分;
        comment: ,      // 书籍评论Url, 必须有 comment() 方法获得评论内容;
    }
]

2.获取书籍信息方法
/**
* @param  url   书籍详情Url或Id
*/
function detail(url: String) {}
@RETURN
@其中, 名称前标有*号的, 是必须存在的;
{
    * catalog: ,// 书籍章节列表地址或ID;
    name: ,     // 书籍名称, 如果未提供, 则使用search获得的书籍名称;
    author: ,   // 书籍作者, 如果未提供, 则使用search获得的作者名称;
    cover: ,    // 书籍封面, 如果未提供, 则使用search获得的封面图;
    summary: ,  // 书籍简介, 如上.
    category: , // 书籍类型, 如上.
    status: ,   // 书籍状态, 统一为 连载, 完结, 如上.
    lastChapter: ,  // 最后章节名, 如上.
    words: ,        // 书籍字数, 如上.
    update: ,       // 更新日期时间戳或字符串, 如上.
    tags: ,         // 书籍tags, 如上.
    score:,         // 书籍得分、评分;, 如上.
    comment: ,      // 书籍评论Url, 必须有 comment() 方法获得评论内容;, 如上.
}

3.获取章节列表方法
/**
* @param  url   书籍章节列表Url或ID
*/
function catalog(url: String) {}
@RETURN
{
    isVip: , // 本章节是否是Vip/收费章节;
    name: ,  // 章节名称
    url:  ,  // 章节地址
    isVolume: , // 是否是卷名? 
}


4.获得章节正文方法
/**
* @param  url   章节Url或Id
* @return String 正文内容;
*/
function chapter(url: String) {}


5.获得分类书籍方法
/**
* @param  title     分类1/大分类/总分类
* @param  category  分类2/小分类/子分类
* @param  page      页索引
* @注意   定义此方法之前, 必须定义分类数据
*/
function rank(title: String, category: String, page: Int) {}
@RETURN:
@其中, 名称前标有*号的, 是必须存在的;
[
    {
        * name: ,   // 书籍名称
        * author: , // 书籍作者
        * detail: , // 书籍URL或地址
        cover: ,    // 书籍封面
        summary: ,  // 书籍简介
        category: , // 书籍类型
        status: ,   // 书籍状态, 统一为 连载, 完结
        lastChapter: ,  // 最后章节名
        words: ,        // 书籍字数
        update: ,       // 更新日期时间戳或字符串
        tags: ,         // 书籍tags
        score:,         // 书籍得分、评分;
        comment: ,      // 书籍评论Url, 必须有 comment() 方法获得评论内容;
    }
]


6.定义书城/分类数据
详情请参考有ranks的JS源;


7.定义书源信息
var bookSource = JSON.stringify({
    name: "xxxx小说",   // 源名称, 可随意, 可重复;
    url: "17k.com",    // 源网站地址, 不可重复, 重复的url,会被视为一个书源;
    authorization: "https://passport.17k.com/login", // 定义登录访问Url, 如果希望使用post/get进行登录, 请参考
    cookies: [".17k.com"], // 定义此源可访问的cookie
    ranks: ranks           // 定义书城分类字段名
})

###authorization 可接受以下三种定义方式;

1.
authorization: "url地址", 此种方式, 将通过WebView(内置浏览器)登录;

2. 不推荐使用这种方式, 不推荐, 不推荐, 不推荐, 非常, 强烈, 特别不推荐~
authorization: [标签1, 标签2] // 此种方式, App将会自动根据标签数量, 创建输入框, 和一个按钮, 当输入内容后, 点击按钮, 就会将所有输入框内容合并为一个对象, 将此对象作为参数, 调用方法xxx

3. 推荐使用这种方式, 推荐, 推荐, 推荐, 非常, 强烈, 特别推荐~
authorization: [
    {
        id: "phone"
        name: "" // 本控件显示名称;
        type: "控件类型" // 控件类型可选 phone，password，text，button，image， 其中, phone与password与text一般，都是输入框,只不过phone会对输入内容做校验，如果不是手机号码，会报错，password会对输入的内容做遮掩处理。button是按钮，image是图像框，用来做图文验证码；
        acation: "本控件所绑定的JS方法名称" // 仅当type是button或image时，才会需要此参数, 点击按钮，会调用所指定的方法;
        bind: [] // 本控件绑定的元素, 当本控件是button时，点击本控件， 会将所绑定的元素，转为对象，传递给action定义的方法;
    }
]
可参考如下定义;

authorization: [
    {
        id: "id_phone",
        type: "phone", 
        name: "手机号码",
    }, {
        id: "id_code",
        type: "text", 
        name: "验证码",
    }, {
        type: "button",
        bind: ["id_phone"],
        name: "获取验证码",
        action: "getPhoneCode" // 调用getPhoneCode方法时, 会传入一个对象参数, 此对象内有id_phone字段, 内容是 id_phone 输入框内容;
    }, {
        type: "button",
        bind: ["id_phone", "id_code"],
        name: "登录",
        action: "login" // 调用login方法时, 会传入一个对象参数, 此对象内有id_phone和id_code字段, 内容分别对应 id_phone和id_code输入框内容;
    }
]