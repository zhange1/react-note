#### webpack优化

###### 1、分类配置文件

- webpack.common.js、webpack.dev.js、webpack.prod.js分别对应通用、开发、生产环境配置，统一放置config文件

###### 2、打包性能分析

- Webpack-bundle-analyzer帮我们分析打包后的依赖包大小

###### 3、打包优化

- 去除大的库中没有用到的代码

  - 例如moment中locale，可以借助moment-locales-webpack-plugin来移除未用到的代码

    ```javascript
    // webpack.common.js
    const MomentLocalesPlugin = require('moment-locales-webpack-plugin');
    
    module.exports = {
      // ...
      plugin: [new MomentLocalesPlugin()]
    }
    ```

- 优化Loader配置

  - loader的include配置，用来制定目录 include: path.resolve(__dirname, '../src')

- 优化resolve.modules配置

  - 使用绝对路径来替代默认方式，减少寻找层级

- 优化resolve.mainFields配置

  - 为了减少寻找步骤，使用'main'来制定描述入口文件位置

- 优化resolve.alias配置

  - 默认情况下，引入'react'会让webpack从入口文件'./node_modules/react/react.js'开始递归解析和处理依赖的几十个文件，这是一个耗时的操作，通过配置resovle.alias可以让webpack在处理react库时，直接使用单独完整的react.min.js，跳过耗时的递归解析操作。
  - 但对于某些库这样使用会影响tree-shaking优化，因为打包好的完整文件里有部分代码项目会永远用不到，但某些整体性比较强的库就可以使用这个方法来优化。

- 优化resolve.extensions配置

  - 在导入语句没带文件后缀时，Webpack 会自动带上后缀后去尝试询问文件是否存在。resolve.extensions 用于配置在尝试过程中用到的后缀列表：extensions: ['.js', '.json']，也就是说当遇到 require('./data') 这样的导入语句时，Webpack 会先去寻找 ./data.js 文件，如果该文件不存在就去寻找 ./data.json 文件，如果还是找不到就报错。如果这个列表越长，或者正确的后缀在越后面，就会造成尝试的次数越多，所以 resolve.extensions 的配置也会影响到构建的性能。 在配置 resolve.extensions 时你需要遵守以下几点，以做到尽可能的优化构建性能：

  - 后缀尝试列表要尽可能的小，不要把项目中不可能存在的情况写到后缀尝试列表中。
    频率出现最高的文件后缀要优先放在最前面，以做到尽快的退出寻找过程。
    在源码中写导入语句时，要尽可能的带上后缀，从而可以避免寻找过程。例如在你确定的情况下把 require('./data') 写成 require('./data.json')。

  - ```javascript
    module.exports = {
      // ...
      resolve: {
        // 尽可能的减少后缀尝试的可能性
        extensions: ['.tsx', '.ts', '.js'],
      },
    };
    ```

    

- 优化module.noParse配置

  - 可以让webpack忽略对部分没有采用模块化的文件的递归解析处理，这样做的好处是能提高构建性能，原因是有一些苦，例如jquery，chartJS它们庞大又没有采用模块化标砖，让webpack去解析这些文件耗时又没意义。

- 保持最新版本

  - 官方一直在改进性能，所以使用最新版本webpack以及node版本也有助于提高性能

- 减少辅助程序

  - 尽量减少不必要使用的loader和plugin

- resolving

  - 如果项目里未使用npm link或者yarn link，可以设置resolve.symlinks：false

###### 4、开发环境优化

- 开启缓存
- Devtool
- 避免使用生产环境才需要的工具
- 最小的入口chunk
- 避免额外的优化步骤
- 输出不带路径信息
- TypeScript loader

###### 5、生产环境优化

- 并行构建
- source Maps

###### 6、Tree Shaking

- 压缩css
- 图片优化



