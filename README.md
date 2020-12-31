# 概述
Scheme是一种页面内跳转协议，通过定义自己的Scheme协议，可以非常方便的跳转APP中的各个页面，Scheme格式如下：
```
[scheme]://[host]/[path]?[query]
```
比如，D豆APP，我们公司定义的Scheme协议为：`ddou://www.d-dou.com` 。具体可参照此形式设计。

为了方便H5开发者使用，特封装 `lg-schemes` 库，预用此库，原生需严格按照如下约定处理Scheme跳转。以D豆为例，大致分为以下4类：

```markdown
1. 原生跳转H5页面
ddou://www.d-dou.com/push
2. H5跳转原生页面
ddou://www.d-dou.com/jump
3. H5跳转原生tab页
ddou://www.d-dou.com/switch
4. 原生通过外部浏览器打开某个H5页面
ddou://www.d-dou.com/browser
```



「具体定义方式如下所示」



**a. 原生跳转H5页面：**

```
ddou://www.d-dou.com/push?url=encodeURIComponent(http[s]://xxx.com?needHeader=?&appBack=?&xxx=xxx)
```

示例解读：

- needHeader：是否需要原生实现导航栏，0为不需要，1为需要；
- appBack：点击导航栏上的返回按钮是否需要调用原生返回，0为不需要，1位需要。

**b. H5跳转原生页面：**

```
ddou://www.d-dou.com/jump/path?xxx=xxx
```

> 说明：`path` 为原生路由，`?xxx=xxx` 为H5传递给原生的数据。在开发中，**原生开发者需拟定原生路由参照表供H5、后台及测试人员使用。**

**c. 切换tab页：**

```
ddou://www.d-dou.com/switch?index=x
```

> 说明：`index` 为原生tab栏上对应的下标，比如首页，`index`  值为 `0`。

**d. 原生打开外部浏览器：**

```
ddou://www.d-dou.com/browser?url=xxx
```

> 说明：`url` 为外部H5链接地址。


# 安装
```shell
$ npm install lg-schemes
# OR
$ yarn add lg-schemes
```

# 使用

使用之前，你需要全局配置scheme
```tsx
import Schemes from 'lg-schemes';
Schemes.config('scheme地址', '二级目录地址');
```
> 提示：
>
> - 如果你的项目部署在二级目录下，还需配置二级目录地址。
>
> - scheme地址通常为Host部分，如D豆，你的配置方式应该是这样的：
>
>   ```js
>   Schemes.config('ddou://www.d-dou.com');
>   ```


具体API如下；

```typescript
interface PushOptions {
    query?: Record<string, any>;
    needHeader?: 0 | 1;
    appBack?: 0 | 1;
}

/**
 * 全局配置项，你应该在项目初始化时调用config进行配置。
 *
 * @param scheme scheme地址，只需要配置前缀，如：ddou://www.d-dou.com
 * @param base 二级目录地址
 */
config(scheme: string, base?: string): void;
/**
 * 跳转H5页面
 *
 * @param path H5路由
 * @param options  可选项
 */
push(path: string, options?: PushOptions): void;
/**
 * 切换原生tab页
 * @param index
 */
switchTab(index: number): void;
/**
 * 跳转原生页面
 *
 * @param path
 */
jump(path: string, params?: Record<string, any>): void;
/**
 * 原生打开外部浏览器
 * @param url 资源地址
 */
openBrowser(url: string): void;
```





