## 自定义 Generator

根据之前 Yeoman 的基础使用，我们发现不同的 Generator 可以生成不同的项目，也就是说我们可以通过自己的 Generator 生成自定义的项目结构。

## 创建 Generator 模块

Generator 本质上就是一个 NPM 模块。

下面是生成器目录结构：

![Generator模块目录结构](https://i.loli.net/2020/11/09/A5vmg3PLKWHuaGl.png)

除了特定的结构，还有一个与普通 NPM 模块不同的是，Yeoman 的 Generator 模块名称必须是 `generator-<name>`，如果在开发过程中没有使用这个格式的名称，Yeoman 在后续工作中就无法找到这个生成器模块。

下面来创建一个 Generator 模块：

```bash
mkdir generator-sample

cd generator-sample/

yarn init

# 安装 yeoman 的一个模块 yeoman-generator，它提供了 Generator 生成器的基类
# 基类中提供了一些的工具函数，让我们在创建生成器的时候更加便捷
yarn add yeoman-generator   

# 安装依赖
yarn
```

打开项目文件目录，按生成器目录格式新建文件夹与文件。编写 Generator 入口文件的逻辑，即可完成一个生成器。

![简单的Generator](https://i.loli.net/2020/11/09/DIFOTWhbi6Bs1Md.png)

然后使用 `yarn link` 将当前 Generator 链接到全局使之成为全局模块包，这样 Yeoman 在工作的时候就可以找到我们自己编写的 generator-sample 了。

下面是 Yeoman 使用上面自定义的生成器：

```bash
mkdir my-project

cd my-project/

# 运行 yo sample 命令就可以按照自定义的 generator-sample 生成器进行工作
yo sample
```

运行命令后会根据按照生成器创建一个 temp.text 文件，并写入随机数字符串。

## 根据模板创建文件

很多时候我们需要创建的文件有很多，而且文件内容也相对复杂，在这样的情况下我们可以使用模板去创建文件。

具体就是在 /app 目录下创建 /templates 目录，然后将需要生成文件都放入 /templates 目录作为模板。

![模板文件](https://i.loli.net/2020/11/09/RvCyL2oGaiwUSbn.png)

有了模板文件之后，我们在生成文件的时候就不需要借助 fs 的 write 方法了，而是借助 fs 的 copyTpl 方法。

![模板文件生成文件](https://i.loli.net/2020/11/09/jKNxXc5w3aZ6yue.png)