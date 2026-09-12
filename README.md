# PI-Desktop 插件分发仓库（AIUO-Net）

这是一个**制品仓库**，不是源码仓库。客户端（PI-Desktop）从这里读取市场索引 `catalog.json`，
并按其中的相对路径从本仓库下载 `packages/*.piplug`。源码在各插件自己的仓库里。

## 目录约定

| 路径 | 内容 | 谁来写 |
| --- | --- | --- |
| `catalog.json` | 市场索引：插件、版本、shasum、相对包路径 | **plugins.aiuo.net 的发布流水线**（勿手改） |
| `packages/<id>-<version>.piplug` | 插件包本体（store 压缩 zip） | 同上 |
| `.gitattributes` | `* -text`：禁止换行转换，否则 shasum 会变 | 平台每次发布会重写 |

## 分支

| 分支 | 用途 |
| --- | --- |
| `main` | 正式分发 |
| `Test` | 测试服（plugins.aiuo.net 的测试实例推这一支） |

发布时平台只会在目标分支上**增加/替换**自己写入的文件；测试分支与正式分支互不影响。

## 客户端读取顺序

1. **平台目录**：`https://plugins.aiuo.net/catalog.json` —— 实时渲染，审批通过即刻可见；
2. **备份目录**：本仓库 raw
   （`https://raw.githubusercontent.com/AIUO-Net/pi-desktop-plugins/main/catalog.json`）；
3. **国内镜像**：CNB（待接入）。

平台只做**上传中转**：索引由平台产生，**包的下载一律从本仓库（或其镜像）进行，不经过平台**。
平台提供的目录会通过 `artifactBaseUrl` 把相对路径解析到本仓库。

## 不要做的事

- 不要手工编辑 `catalog.json`：下一次发布会被整份覆盖。
- 不要手工删除 `packages/` 下的文件：客户端按目录里的 shasum 校验，缺文件会让对应版本无法安装。
- 不要把 `Test` 分支合并进 `main`：两边的目录指向各自分支上的包。
