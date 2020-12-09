# 概述
scheme跳转协议为H5和原生之间的跳转协议，命名规范如下：
```
protocol://host
```
如，D豆项目定义的scheme协议为：`ddou://www.d-dou.com`，具体可参照此形式设计。

目前根据实际开发的场景定义如下跳转形式（示例以D豆为例），如果需要使用该库，原生需严格按照如下约定处理scheme跳转。

**a. 原生跳转H5页面：**

```
 ddou://www.d-dou.com/push?url=encodeURIComponent(url=xxx?needHeader=?&appBack=?)
```

示例解读：

- needHeader：是否需要原生实现导航栏，0为不需要，1为需要；
- appBack：点击导航栏上的返回按钮是否需要调用原生返回，0为不需要，1位需要。

**b. H5跳转原生页面：**

```
ddou://www.d-dou.com/jump/path?xxx=xxx
```

> 说明：`path` 为原生路由，`query` 参数为传递给原生的数据。在开发中，**原生开发者需拟定原生路由参照表供H5、后台及测试人员使用。**

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

为了便于开发使用，这里封装lg-schemes库。使用之前，你需要全局配置scheme
```
import Schemes from 'lg-schemes';
Schemes.config('scheme地址', '二级目录地址');
```
> 提示：如果你的项目部署在二级目录下，还需配置二级目录地址，否则不传。


具体API如下；

```typescript
/**
 * 全局配置项，你应该在项目初始化时调用config进行配置。
 * 
 * @param scheme scheme地址，只需要配置前缀，如：ddou://www.d-dou.com
 * @param base 二级目录地址
 */
config(scheme: string, base?: string): void;
/**
 * 原生跳转H5页面
 * @param path H5路由
 * @param options  可选项
 */
push(path: string, options: PushOptions): void;
/**
 * 切换原生tab页
 * @param index
 */
switchTab(index: number): void;
/**
 * H5跳转原生页面
 * @param path
 */
jump(path: string, params?: Record<string, any>): void;
/**
 * 原生打开外部浏览器
 * @param {string} url 资源地址
 */
openBrowser(url: string): void;
```





