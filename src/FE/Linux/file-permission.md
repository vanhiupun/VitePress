---
title: Linux 文件权限
head:
  - - meta
    - name: keywords
      content: Linux
  - - meta
    - name: description
      content: Linux 文件权限
---

## Linux 的文件权限

Linux 操作系统中对权限的管理很严格。Linux 系统中不仅是对用户与组根据 UID，GID 进行了管理，还对 Linux 系统中的文件，按照用户与组进行分类，针对不同的群体进行了权限管理，用他来确定谁能通过何种方式和目录进行访问和操作。

权限共有 10 个字符，我们将它分为 4 大部分来理解：

**表示文件的类型**
|||
|---|---|
|`-`|表示是一个文件|
|`d`|表示是一个目录|
|`l`|表示是一个连接（理解为快捷方式）|

**用户组类型**
|缩写|用户组|类型|
|---|---|---|
|`u`|`owner`| 属主 :当前用户具有的对该文件的权限|
| `g`|`group`|当前组内其他用户具有对该文件的权限|
|`o`|`other`|其他组的用户具有的对该文件的权限| -其他组的用户具有的对该文件的权限

**权限**

- `r`：`Read` 读
- `w`：`Write` 写
- `x`：`execute` 执行

针对目录加执行权限，文件不加执行权限（因文件具备执行权限有安全隐患）

针对文件和目录来说，`r，w，x`有着不同的作用和含义
|命令 |针对文件|针对目录|
| --- | --- | --- |
|`r`|读取文件内容|查看目录下的文件列表|
|`w`|修改文件内容|删除和创建目录下的文件|
|`x`|执行权限对除二进制程序以外的文件没什么意义|可以 cd 进入目录，能查看目录中文件的详细属性，能访问目录下文件内容(基础权限)|

:::tip
root 账户不受文件权限的读写限制，执行权限受限制
:::

<table data-draft-node="block" data-draft-type="table" data-size="normal" data-row-style="normal"><tbody><tr><td>权限项</td><td>文件类型</td><td>读</td><td>写</td><td>执行</td><td>读</td><td>写</td><td>执行</td><td>读</td><td>写</td></tr><tr><td>字符表示</td><td>(d|l|c|s|p)</td><td>（r）</td><td>（w）</td><td>(x)</td><td>(r)</td><td>(w)</td><td>(x)</td><td>(r)</td><td>(w)</td></tr><tr><td>数字表示</td><td></td><td>4</td><td>2</td><td>1</td><td>4</td><td>2</td><td>1</td><td>4</td><td>2</td></tr><tr><td>权限分配</td><td></td><td>文件所有者</td><td>文件所属组用户</td><td>其他用户</td></tr></tbody></table>

用户获取文件权限的顺序：先看是否为所有者，如果是，则后面权限不看；再看是否为所属组，如果是，则后面权限不看。

## 修改文件权限

`chown` 是 `change owner` 的意思，主要作用就是改变文件或者目录所有者。
|||
|---|---|
|`chmod`|修改文件和文件夹读写执行属性。使用权限：`所有使用者`|
|`chown`|修改文件和文件夹的用户和用户组属性。使用权限：`root`|

## mode 方式

```bash
chmod who opt per file
```

|       |                                                                                   |
| ----- | --------------------------------------------------------------------------------- |
| `who` | ` u g o a(all)` (`u 用户 user`，`g 用户组 group`，`o 其他用户`，`a 所有用户默认`) |
| `opt` | `+`添加某个权限 `-`取消某个权限 `=`赋予权限                                       |
| `per` | `r w x X`                                                                         |

示例：

```bash
chmod u=rwx,g=r a.txt
chmod u+x,g+w,o+w test.log #r 读，w 写， x 执行
```

## 数字方式

语法：`chmod xxx file`
||||
|---|---|---|
|`rwx`| `421` |`7`|
|`rw-`| `420`| `6`|
|`r--` |`400` |`4`|

- `0` 表示无权限，
- `1` 表示可执行`x`，
- `2` 表示写入权限`w`，
- `4` 表示可读权限`r`

**例如：**

```bash
-rwxr--r-- 1 root root 10 oct 16 02:55 yhp.log
```

| 权限         | 参数  | 参数    | 参数 |
| ------------ | ----- | ------- | ---- |
| 用户权限     | `rwx` | `4+2+1` | `7`  |
| 所属组权限   | `r--` | `4+0+0` | `4`  |
| 其他用户权限 | `r--` | `4+0+0` | `4`  |

**组合：744**

**修改权限：**

- 都加入写入权限 +2
- 给用户组加入写入权限：+2
- 给其他用户加入可执行权限：+1

**组合：765**

```bash
chmod 765 a.txt
```