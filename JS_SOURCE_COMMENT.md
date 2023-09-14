# 书源评论, 请参考本模板;
    1.comment 必须
    2.replyComment 非必须

## `comment(book, commentId, page) : []`<br>
>`本方法是加载章节段落评论方法;`<br>
>`*  book 由 catlog 返回的 url 字段, 此字段应为json, 且必须具有 bookId, chapterId这两个记录, 字段名无做限制, 由书源自行决定;`<br>

### PARAM
>* ` book              包含书籍ID与章节ID的JSON`<br>
>* ` commentId         对应评论ID或段落索引`<br>
>* ` page              加载评论页数`<br>


### RETURNS
>* ` id                发表ID`<br>
>* ` userName          发表用户名`<br>
>* ` userImg           发表用户头像`<br>
>* ` content           发表评论内容`<br>
>* ` date              发表评论日期`<br>
>* ` digg              此发表或的同意数量`<br>
>* ` disagree          此发表获得反对数量`<br>
>* ` local             发表位置或IP`<br>
>* ` image []          发表图像数组`<br>
>* ` replyCount        此发表内容回复数量, 只有此值大于 0 或 replys 不为空数组 时, 才显示展开加载回复布局`<br>
>* ` replys [] or id   此发表回复数据, 如果是数组, 则直接加载显示本数组内容, 如果是单个字符串, 则将其视为评论ID, 将调用 "replyComment" 获得回复数据`<br>

### 举个栗子
假设 评论数据如下(本假设不针对任何App或网站的数据, 完全由本人凭空捏造)：

    {
        status: "success",
        Data: {
            List: [
                {
                    content: "你好啊, 我是评论数据"
                    userName: "balabala"
                    userIcon: "https://www.xxx.xx/xxxx图片"
                    Id: 11223344 // 挺重要的, 需要靠此ID来获得本条评论的回复列表;
                    .....
                }, {
                    content: "你好啊, 我是第二条评论数据"
                    userName: "啦啦啦"
                    userIcon: "https://www.xxx.xx/xxxx图片"
                    Id: 44332211 // 挺重要的, 需要靠此ID来获得本条评论的回复列表;
                    ...
                }
            ]
        }
    }


#### CODE
    function comment(book, commentId, page) {
        let bid = JSON.parse(book).bid;
        let cid = JSON.parse(book).cid;

        // 假设请求地址;
        let response = HTTP操作(url + `bookId=${bid}&chapterId=${cid}&page=${page}&comment=${commentId}`)
        let $ = JSON.parse(response).Data
        let array = []
        for (let item of $.List) {
            array.push({
                // 为了防止Id数值过大, 造成精度确实, 所以使用BigInt将其转为String;
                id       :   BigInt(item.Id).toString(),
                userName :   item.userName,
                userImg  :   item.userIcon,
                content  :   item.content,
                .....
                replys   :   BigInt(item.Id).toString()
            })
        }

        return JSON.stringify(array);
    }


## `replyComment(book, commentId, page, paragraph) : []`<br>
>`本方法是加载指定评论的所有回复列表;`<br>
>`*  book 由 catlog 返回的 url 字段, 此字段应为json, 且必须具有 bookId, chapterId这两个记录, 字段名无限制, 由书源自行决定;`<br>

### PARAM
>* ` book              包含书籍ID与章节ID的JSON`<br>
>* ` commentId         对应评论的ID`<br>
>* ` page              加载评论页数`<br>
>* ` paragraph         此评论对应的段落索引`<br>
>* ` 以上参数并非都有用,视情况而定;`


### RETURNS
>* ` replyUserName     回复用户名, 指 此条评论是回复给哪个用户的`<br>
>* ` userName          发表用户名`<br>
>* ` userImg           发表用户头像`<br>
>* ` content           发表评论内容`<br>
>* ` date              发表评论日期`<br>
>* ` digg              此发表或的同意数量`<br>
>* ` disagree          此发表获得反对数量`<br>
>* ` local             发表位置或IP`<br>
>* ` image []          发表图像数组`<br>

### 举个栗子
假设 评论数据如下(本假设不针对任何App或网站的数据, 完全由本人凭空捏造)：

    {
        status: "success",
        Data: {
            List: [
                {
                    reply_user_name: "蟹阿金"
                    content: "好人一生平安"
                    userName: "balabala"
                    userIcon: "https://www.xxx.xx/xxxx图片"
                    Id: 11223344 // 挺重要的, 需要靠此ID来获得本条评论的回复列表;
                    .....
                }, {
                    reply_user_name: "海绵宝宝"
                    content: "好, 你说的不错"
                    userName: "啦啦啦"
                    userIcon: "https://www.xxx.xx/xxxx图片"
                    Id: 44332211 // 挺重要的, 需要靠此ID来获得本条评论的回复列表;
                    ...
                }
            ]
        }
    }


#### CODE
    function replyComment(book, commentId, page, paragraph) {
        let bid = JSON.parse(book).bid;
        let cid = JSON.parse(book).cid;

        // 假设请求地址;
        let response = HTTP操作(url + `bookId=${bid}&chapterId=${cid}&page=${page}&comment=${commentId}&paragraph=${paragraph}`)
        let $ = JSON.parse(response).Data
        let array = []
        for (let item of $.List) {
            array.push({
                reply_user_name:  item.reply_user_name,
                userName       :  item.userName,
                userImg        :  item.userIcon,
                content        :  item.content,
            })
        }

        return JSON.stringify(array);
    }


