<a name="VEAFZ"></a>
## 版本检查[HELLO]
允许已授权的用户通过此接口更新自己的信息。
:::tips

- **URL**：`/api/hello`
- **Method**：`GET`
:::
<a name="egiB4"></a>
### 成功响应
:::tips
**状态码：**`200 OK`
:::
```json
{
    "code": 200,
    "msg": "Welcome",
    "appVer": int,
    "portVer": int,
    "name": Device Name
}
```
<a name="SjKLo"></a>
### 错误响应
:::tips
**状态码**：`404`
:::
```json
{"code": 404, "msg": "不受支持的操作"}
```

<a name="Mwjl1"></a>
## <br />
