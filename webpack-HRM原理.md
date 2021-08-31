### HMR工作流程分析

webpack监听到项目中文件或者模块的代码有修改后，由webpack.HotModuleReplacementPlugin插件生成hot-update.json和hot-update.js补丁文件，然后客户端通过socket连接得到补丁文件，最后由
HMR runtime将补丁文件的内容应用到模块系统中。

### HMR工作原理图

![webpack hmr工作原理图](http://ttc-tal.oss-cn-beijing.aliyuncs.com/1630319394/hmr.png)
上图展示了从我们修改代码，到模块热更新完成的一个 HMR 完整工作流程，图中已用箭头将流程标识出来。

1. webpack-dev-server提供http server服务和socket server服务，在webpack-dev-middleware中间件中启用webpack的watch模式。
2. 当文件系统中某一个文件发生修改，webpack 监听到文件变化，根据配置文件对模块重新编译打包，并将打包后的代码保存在文件系统中。
3. webpack-dev-server/client/index.js在浏览器端和服务端之间建立一个 websocket长连接，将webpack编译打包的状态信息告知浏览器端。
4. 浏览器端通过webpack/hot/dev-server.js接收到最新的编译信息，从而触发module.hot.check方法来开始热更新检查。
5. 执行webpack/lib/hmr/HotModuleReplacement.runtime.js中的hotCheck方法，通过hmrDownloadManifest以fetch方式请求hot-update.json文件，然后执行loadScript以jsonp的方式加载hot-update.js文件。
6. 执行hot-update.js文件中的self[‘webpackHotUpdate’]方法，在webpack/lib/web/JsonpChunkLoadingRuntimeModule.js文件的languageself[‘webpackHotUpdate’]方法定义中去收集到需要更新的模块依赖。
7. 调用internalApply方法，用新模块替换掉旧模块，然后执行module.hot.__acceptedDependencies中的回调函数，从而实现模块替换的效果。