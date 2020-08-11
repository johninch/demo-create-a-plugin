# demo-create-a-plugin
学习实现 webpack plugin，这里实现一个生成版权信息文件的插件 CopyrightWebpackPlugin

```js
    apply(compiler) {
        // compiler.hooks，常用如下：
            // compile: SyncHook同步的，一个新的编译(compilation)创建之后，钩入(hook into) compiler
            // compilation: SyncHook同步的， 编译(compilation)创建之后，执行插件
            // emit: AsyncSeriesHook异步的， 生成资源到 output 目录之前
            // done: SyncHook同步的， 编译(compilation)完成
        // 同步hook用tap，异步hook用tapAsync
        compiler.hooks.compile.tap('CopyrightWebpackPlugin', compilation => {
            console.log('compile是同步hook')
        })
        compiler.hooks.emit.tapAsync('CopyrightWebpackPlugin', (compilation, cb) => {
            // debugger;
            // compiler 存放了所有配置和打包的内容
            // compilation 只存放当前这次打包相关的内容
            // compilation.assets 存放了所有打包生成的内容
            compilation.assets['copyright.txt'] = {
                source: () => {
                    return 'copyright by John Inch'
                },
                size: () => {
                    return 21; // source return 的字符长度
                }
            }
            cb() // 使用tapAsync异步方式，必须触发下回调
        })
    }

```
