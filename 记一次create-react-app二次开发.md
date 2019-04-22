1. 脚手架的工作原理
2. 为什么我们要从v1.1.5升级到v2.1.8
>
- 新版本支持提供额外本地模板，这使我们创建多模板时不需要在全局安装新的全局命令
- 重新定义我们的多入口开发模式，带来新的开发体验，提高开发效率
- 新版本支持typesrcipts
- 新版本的代码更加精简，代码更加好管理，这也是webpack4带来的优势，不需要定义开发模式和生产模式这样两份配置文件。
- webpack3 -> webpack4
3. [ webpack4的新特性](https://github.com/webpack/webpack/releases?after=v4.2.0)
> 
- 零配置(实际就是增加了很多默认配置项，让我们在无需配置config文件的时候直接使用，比如默认entry，output,mode)
- 废弃CommonsChunkPlugin，UglifyJsPlugin，增加配置项optimization，这个功能很强大，稍后4中讲解
- extract-text-webpack-plugin ->mini-css-extract-plugin
- 提高打包速率(180%)
- browserslist
4. 解读一下我们的webpack配置文件
> 
- [pnp](https://bobi.ink/2019/04/08/plug-n-play/) yarn的 新型模块加载方式
- [WorkboxWebpackPlugin](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin)离线应用pwa生成方案
- [cssnano](https://cssnano.co/) css tree shaking
- vw,vh的适配方案
- 代码分割，代码懒加载
- 多入口配置
5. 二次开发我们做了哪些优化
>
-  vw方案的适配
-  增加了less的支持
-  增强proxy的代理方案
-  修改了cssnano方案
-  增加了svg sprite
-  支持多入口
-  支持antd-mobile css按需加载
-  支持装饰器@
-  去除react-app-polyfill,使用自定会polyfill,扩大babel-polyfill的兼容范围
- 自定义jinkens打包命令
- 接入微信分享，性能监控等公用sdk
6.遗留的问题
> 
- wetime-mobile的tree shaking无效

