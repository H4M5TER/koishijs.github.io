---
sidebarDepth: 2
---

# 编写和查看帮助

::: tip 提示
阅读本章前，你最好先对 Koishi 的 [**指令系统**](../../guide/command-system.md) 有一个系统的认识。
:::

## 查看帮助

Koishi 拥有着强大的指令系统，然而过于复杂的功能也会困扰使用者。因此，Koishi 也内置了 help 指令，用于输出全部或特定指令的使用方法。你可以这样调用它：

```sh
help                # 显示一级指令列表
help echo           # 显示 echo 指令的信息
echo -h             # 显示 echo 指令的信息（与上面相同，且 -h 可以替换成 --help）
```

这里的信息可以包含很多内容，比如指令的名词，参数，选项，子指令，权限设置等等。

此外，尽管上面的 echo 是指令名，但是也可以换成别名甚至快捷调用，help 指令照样可以识别。

### 查看子指令

在 [子指令](../../guide/command-system.md#子指令) 一节中我们已经透露了 help 指令在查看子指令方面的行为。让我们先回顾一下 Koishi 支持的两种子指令格式。一种是**层级式**（例如 foo/bar），另一种则叫**派生式**（例如 foo.bar）。它们在使用上有所不同，层级式可以直接通过最后一级进行调用，而派生式则需要写全指令名。 

尽管有这些不同，但是上述两个指令都属于 foo 的子指令，因此它们

1. 都不会显示在 `help` 的输出中（因为它们都不是一级指令）
2. 都会显示在 `help foo` 的输出中（因为它们都是 foo 的子指令）

但是，我们又知道，foo/bar 是可以直接作为 bar 来调用的，而 foo.bar 不行，那么有没有一种方法可以插件全部可以直接调用的指令列表呢？Koishi 也提供了方法（这里的 -e 是 --expand 的缩写）：

```sh
help -e             # 显示全部可以直接调用的指令列表
help baz -e         # 显示指令 baz 的信息，以及全部可以直接调用的 baz 的子指令列表
```

### 查看快捷方式

我们已经学会了搜索特定指令，特定快捷方式和搜索全部指令，那么自然也有搜索全部快捷方式的方法（这里的 -s 是 --shortcut 的缩写）：

```sh
help -s             # 显示全部指令的快捷方式列表（无论是不是一级指令）
```

## 编写帮助

::: tip 提示
下面的 echo 指令是为了理解方便而举的例子，与真实的内置 echo 指令可能不同。
:::

之前已经介绍了 `ctx.command()` 和 `cmd.option()` 这两个方法，它们都能传入第二个字符串作为参数。这个字符串会作为这个指令或选项的描述文字。就像这样：

```js
ctx.command('echo <message...>', '输出收到的信息')
  .option('-t, --timeout <seconds>', '设定延迟发送的时间')
```

那么这些内容如何才能让用户看到呢？别担心，koishi-plugin-common 中内置了一个强大的 help 指令，可以让你方便地查看一个指令的帮助文档。调用 `help echo` 或者 `echo -h`（这个默认情况下会被 Koishi 自动转化为 help 指令调用），你会看到这样的回复：

```
echo <message...>
输出收到的信息
可用的选项有：
    -t, --timeout <seconds>  设定延迟发送的时间
```

### 添加用法和使用示例

当然，我们还可以加入具体的用法和使用示例，进一步丰富这则使用说明：

```js
ctx.command('echo <message...>', '输出收到的信息')
  .option('-t, --timeout <seconds>', '设定延迟发送的时间')
  .usage('注意：参数请写在最前面，不然会被当成 message 的一部分！')
  .example('echo -t 300 Hello World  五分钟后发送 Hello World')
```

这时再调用 `help echo` 或者 `echo -h`，你便会发现使用说明中已经添加了你刚刚的补充文本：

```
echo <message...>
输出收到的信息
注意：参数请写在最前面，不然会被当成 message 的一部分！
可用的选项有：
    -t, --timeout <seconds>  设定延迟发送的时间
使用示例：
    echo -t 300 Hello World  五分钟后发送 Hello World
```

最后，如果直接调用 `help`，输出的会是全部指令组成的列表。

### 手动禁用 -h, --help 选项

如果你编写的某个指令希望把 -h, --help 用作其他目的，你也可以在手动禁用内置的选项：

```js
ctx.command('echo <message...>', '输出收到的信息', { noHelpOption: true })
```

::: tip 注意
作为内置选项，仅用 `cmd.removeOption()` 方法虽然能够将 -h, --help 的帮助删除，但是不能改变调用指令时显示帮助的行为。你应当确保通过 `noHelpOption` 来控制这个行为。
:::
