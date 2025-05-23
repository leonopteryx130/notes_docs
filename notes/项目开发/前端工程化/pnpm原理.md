# pnpm原理

### pnpm是什么

pnpm 是一个高效的包管理工具，它通过[硬链接和符号链接](../../计算机科学/操作系统/软连接和硬链接.md)的方式，将项目的依赖包存储在一个全局的 store 中，从而避免了重复下载和安装依赖包，大大提高了项目的构建速度和磁盘空间的利用率。

### pnpm的工作原理

pnpm 的工作原理可以分为以下几个步骤：

1. 解析 package.json 文件：pnpm 会解析项目的 package.json 文件，获取项目的依赖包信息。
2. 创建 store 目录：pnpm 会创建一个全局的 store 目录，用于存储所有的依赖包。
3. 下载依赖包：pnpm 会根据项目的依赖包信息，从 npm 仓库中下载对应的依赖包，并将其存储在 store 目录中。
4. 创建符号链接：pnpm 会根据项目的依赖包信息，在项目的 node_modules 目录中创建符号链接，指向 store 目录中的对应依赖包。
5. 构建依赖树：pnpm 会根据项目的依赖包信息，构建一个依赖树，用于描述项目的依赖关系。
6. 安装依赖包：pnpm 会根据依赖树，安装项目的依赖包，并将其存储在项目的 node_modules 目录中。

## pnpm的优势

1. 高效：pnpm 通过硬链接和符号链接的方式，避免了重复下载和安装依赖包，大大提高了项目的构建速度和磁盘空间的利用率。
2. 安全：pnpm 通过 store 目录的方式，将所有的依赖包存储在一个全局的位置，避免了依赖包的污染和冲突。

## pnpm的缺点

1. 兼容性：因为软链接的存在，pnpm 的兼容性可能不如 npm 和 yarn。