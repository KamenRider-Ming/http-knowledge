## URI

URI 在于I(Identifier)是统一资源标示符，可以唯一标识一个资源，是一种字符串文本标准，或者说是一种结构上的概念。

## URL

URL在于L(Locater)，一般来说（URL）统一资源定位符，可以提供找到该资源的路径，由此可见URL也可以标识一个资源，所以URL是URI的子集。

简单来说，URI定义某事物的身份（抽象概念，比如：CEO），而URL提供查找该事物的方法（具体概念：比如谁是CEO）。

往往表示一个具体的访问路径时我们都是使用的URL，使用解析库时使用的是URI，因为这个时候是表达的是一个抽象的概念所以使用URI会比较合适。

## RFC 3986 规范

RFC 3986地址：https://tools.ietf.org/html/rfc3986#section-3.3

### 语法组成

**URI构造**

scheme: 应用层协议，在URN又被称为命名空间  
authority: 所有者，一般是ip:port或者域名  
path: 访问路径  
query: 查询参数  
fregment：片段  

```
    foo://example.com:8042/over/there?name=ferret#nose
    \_/   \______________/\_________/ \_________/ \__/
    |           |            |            |        |
scheme     authority       path        query   fragment
    |   _____________________|__
    / \ /                        \
    urn:example:animal:ferret:nose
```

**Path 组成规则**

为了对 Path 组成规则作出一个比较规范的限制，RFC中衍生出了5种规则，如下，其中`[]`表示可选，`*()`表示最少匹配 0 次，`1*`表示最少要匹配一次，
```
path          = path-abempty    ; 以 "/" 开头 或 为空
            / path-absolute   ; 以 "/" 开头 但不能为 “//”
            / path-noscheme   ; 不以 ":" 开头
            / path-rootless   ; 有效字符串
            / path-empty      ; 空

// 正则规则
path-abempty  = *( "/" segment )
path-absolute = "/" [ segment-nz *( "/" segment ) ]
path-noscheme = segment-nz-nc *( "/" segment )
path-rootless = segment-nz *( "/" segment )
path-empty    = 0<pchar>
segment       = *pchar
segment-nz    = 1*pchar
segment-nz-nc = 1*( unreserved / pct-encoded / sub-delims / "@" ) ; 不包含`:`的任意非零字符串

pchar         = unreserved / pct-encoded / sub-delims / ":" / "@"
unreserved(非保留字符)= APLHA / DIGIT / "-" / "_" / "." / "~"
pct-encoded(pct百分号编码) = %XX
sub-delims = "!" / "$" / "&" / "'" / "(" / ")" / "*" / "+" / "=" / ";" / ","
```

URI由几个层次顺序分明的组件所组成，依次为：scheme, authority, path, query, fragment。
```
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]

hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

### URI引用

URI引用经常用来表示一个资源，URI引用分为两种：URI和相对引用，当URI引用中不匹配以冒号为结束的前缀时（即不包含scheme）为相对引用，否则为URI。
```
URI-reference = URI / relative-ref
```

### 相对引用

```
relative-ref  = relative-part [ "?" query ] [ "#" fragment ]

relative-part = "//" authority path-abempty
            / path-absolute
            / path-noscheme
            / path-empty
```
1. 以双斜杆开头的引用被称为`network-path reference`（网络路径引用），又经常被叫做相对协议URL，本身很少被用，现在用来解决HTTP迁移HTTPS中所产生的兼容性问题，但并不是一个合理的设计。
2. 以单斜杆开头的引用被称为`absolute-path reference`（绝对路径引用）。
3. 不以单斜杆开头的引用被称为`relative-path reference`（相对路径引用）。
