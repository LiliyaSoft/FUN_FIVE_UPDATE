# 编写JS源时,请参考本模板;

## 源基本方法, 以下四个方法, 缺少一个，都是无效源;
    1.search
    2.detail
    3.catalog
    4.chapter

## 源基本方法

### 1. 搜索方法
    /**
    * @param  key   搜索内容
    * @param  pageIdx 页数索引 （如果书源不支持搜索下一页, 忽略此参数即可）, 此参数默认从0开始 ;
    */
    function search (key: String, pageIdx: Int) {  }
    @RETURN:
    @其中, 名称前标有*号的, 是必须存在的;
    {
        "books": 
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
            }
        ], 

        "end": Boolean  // 是否支持下一页?  如果返回的books为empty，则默认将次字段设为false;
    }
    @OR
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
        }
    ]       
    
### 2.获取书籍信息方法
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
    }

### 3.获取章节列表方法
    /**
    * @param  url   书籍章节列表Url或ID
    */
    function catalog(url: String) {}
    @RETURN
    {
        vip: ,    // 本章节是否是Vip/收费章节;
        *name: ,  // 章节名称
        *url:  ,  // 章节地址, 虽然标记有星号, 但是可以空;
        volume: , // 是否是卷名? 
        words: ,  // 本章节字符数量
        update: , // 本章节更新时间
        buy: ,    // 是否已购买本章节? 默认为false, 当vip为false时, App忽略此字段(vip章节会在列表中加一个小锁图标, 当此字段为true时, 会显示解锁图标);
    }


### 4.获得章节正文方法
    /**
    * @param  url   章节Url或Id
    * @return String 正文内容;
    */
    function chapter(url: String) {}
    @RETURN
    {
        *content: , //正文内容
        msg: ,//章节附加内容, 如 作者说明, 显示在章节末尾;
        font: "字体文件名", // 如果本章节需要特定的字体文件进行解密, 则需要指定此字段; JS源自行完成字体的下载;  

        button: , // 当章节加载失败时, 可以添加此字段, 
        method: , // 点击按钮后,执行JS方法; App执行此方法时, 会将当前的书籍章节ID传入;  method支持JS方法, url:http/https:, uri:com.xxx.xxx.xxActivity
        // 如 buyChapter(chapterId)
    }

    // 如 本章节是付费章节, 需要购买后阅读,
    {
        content: "本章节是付费章节, 需要购买后才能正常阅读", 
        button : "点击购买",
        method : "buyChapter"
    }

    // 如 非网络问题导致的加载失败
    {
        content: "正文加载失败, 点击重试", 
        button : "重新加载",
        method : "chapter"
    }

    @OR
    章节内容字符串

### 5.获得分类书籍方法
    /**
    * @param  title     分类1/大分类/总分类
    * @param  category  分类2/小分类/子分类
    * @param  page      页索引
    * @注意   定义此方法之前, 必须定义分类数据
    */
    function rank(title: String, category: String, page: Int) {}
    @RETURN:
    @其中, 名称前标有*号的, 是必须存在的;
    {
        end: ,// 是否已加载到最后一页?
        books: [
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
            }, ...
        ]
    }


### 6.定义书城/分类数据
    详情请参考有ranks的JS源;
    类型1, 书城支持一级二级分类,
    ranks = [
        {
            title: { // 一级分类
                key: xx,   // 分类参数
                value: "分类1"  // 分类名称 
            }
            categories: [ // 二级分类数组
                {
                    key: "xxx",
                    value: "分类1" // 分类名
                }, { 
                    key: "xxx",
                    value: "分类2" // 分类名
                }
            ]
        }
    ]


    类型1, 仅定义一级分类,
    ranks = [
        {
            title: { // 一级分类
                key: xx,   // 分类参数
                value: "分类1"  // 分类名称 
            }
        }, {
            title: { // 一级分类
                key: xx,   // 分类参数
                value: "分类2"  // 分类名称 
            }
        }
    ]

### 7.定义书源信息
    var bookSource = JSON.stringify({
        group: "xxx分组",   // 定义默认分组;
        name: "xxxx小说",   // 源名称, 可随意, 可重复;
        url: "17k.com",    // 源网站地址, 不可重复, 重复的url,会被视为一个书源;
        authorization: "https://passport.17k.com/login", // 定义登录访问Url, 如果希望使用post/get进行登录, 请参考
        cookies: [".17k.com"], // 定义此源可访问的cookie
        ranks: ranks           // 定义书城分类字段名
    })

### authorization 可接受以下三种定义方式;

    1.最最最最推荐此种方式;
    authorization: "url地址", 此种方式, 将通过WebView(内置浏览器)登录;

    2. 不推荐使用这种方式, 不推荐, 不推荐, 不推荐, 非常, 强烈, 特别不推荐~
    authorization: [标签1, 标签2] // 此种方式, App将会自动根据标签数量, 创建输入框, 和一个按钮, 当输入内容后, 点击按钮, 就会将所有输入框内容合并为一个对象, 将此对象作为参数, 调用方法xxx

    3. 推荐使用这种方式, 推荐, 推荐, 推荐, 非常, 强烈, 特别推荐~
    authorization: [
        {
            id: "phone" // 为本组件分配唯一ID, 不得与 authorization 内的其他id重复, 仅输入框(text, phone, email, password)需要定义有效的id;
            name: "" // 本控件显示名称;
            type: "控件类型" // 控件类型可选 phone，password，text，button，image， 其中, phone与password与text一般，都是输入框,只不过phone会对输入内容做校验，如果不是手机号码，会报错，password会对输入的内容做遮掩处理。button是按钮，image是图像框，用来做图文验证码；
            action: "本控件所绑定的JS方法名称" // 仅当type是button时，才会需要此参数, 点击按钮，会调用所指定的方法;
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
            name: "获取验证码", // 按钮标题
            time: 60, // 按钮点击间隔;
            action: "getPhoneCode" // 调用getPhoneCode方法时, 会传入一个对象参数, 此对象内有id_phone字段, 内容是 id_phone 输入框内容;
        }, {
            type: "button",
            bind: ["id_phone", "id_code"], // 绑定上面两个输入框的id;
            name: "登录",
            action: "login" // 调用JS中的login方法, 调用login方法时, 会传入一个对象参数, 此对象内有id_phone和id_code字段, 内容分别对应 id_phone和id_code输入框内容;
        }, {
            type: "image",
            name: "验证码框框",
            action: "验证码地址"
        }
    ]

    支持的type类型有：
    1. text(字符输入框), phone(号码输入框), email(email输入框), password(密码输入框, 有遮掩符号), 必须定义有效的id
    2. image, 必须定义有效的action
    3. button, 必须定义有效的bind 与 action;


## 拓展方法(非必须)
    1.profile(): String  // 获得账户信息;
    2.rank(分类1, 分类2, 页索引:Int)  // 获得书城(或称 发现)书籍列表; 返回数据与search一致;
    3.comment/replyComment; 获得书籍章节评论列表, 内容过多, 另一页展示; 
### [评论内容文档](./JS_SOURCE_COMMENT.md)