在 Linux 中，tty 命令用于显示当前终端设备的名称（即当前用户登录的终端）。tty 是 "teletypewriter"（电传打字机）的缩写，现在泛指终端设备。<br>
/dev/pts/*：表示伪终端（pseudo-terminal），通常是通过 SSH 或图形终端模拟器（如 GNOME Terminal、Konsole）连接的终端。
<br><br>

ttyd 是一个开源的命令行工具，可以将终端会话通过 Web 浏览器共享，类似于 gotty 或 wetty。它基于 C 语言开发，性能较好，支持跨平台运行（Linux/macOS/Windows）<br>
-W（或 --writable）是 ttyd 的一个选项，用于允许客户端（浏览器）向终端写入数据。默认情况下，ttyd 以只读模式运行，即用户可以在浏览器中查看终端输出，但不能输入命令。加上 -W 后，用户可以在 Web 终端中执行交互式命令。<br>
`ttyd -W bash` <br><br>

kitty icat 是 Kitty 终端模拟器 提供的一个功能强大的命令，用于在终端中直接显示图片、动画甚至 PDF 文件，而无需依赖外部图片查看器。它利用 Kitty 的图形协议（Graphics Protocol）在终端内渲染图像，支持多种格式并具有高效显示能力。<br>
`kitty icat image.jpg`<br>

Vim 的 + 寄存器（或 * 寄存器）对应系统剪贴板。
步骤：

进入普通模式（按 Esc 确保不在插入模式）。

复制全部内容到系统剪贴板：

vim
复制
:%y+
% 表示整个文件。

y 是复制（yank）命令。

+ 表示系统剪贴板寄存器。



