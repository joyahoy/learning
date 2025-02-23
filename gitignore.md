`.gitignore` 文件用于指定 Git 应忽略的文件和目录，避免将它们纳入版本控制。以下是 `.gitignore` 文件的基本语法和用法：

### 1. 基本规则
- **忽略文件**：直接写文件名，如 `file.txt`。
- **忽略目录**：以 `/` 结尾，如 `dir/`。

### 2. 通配符
- `*`：匹配任意字符（除 `/` 外），如 `*.log` 忽略所有 `.log` 文件。
- `?`：匹配单个字符，如 `file?.txt` 忽略 `file1.txt`、`file2.txt` 等。
- `**`：匹配任意层级的目录，如 `**/temp/` 忽略所有 `temp` 目录。

### 3. 注释
- 以 `#` 开头的行是注释，如 `# 这是一个注释`。

### 4. 否定规则
- 以 `!` 开头表示不忽略，如 `!important.log` 不忽略 `important.log`。

### 5. 示例
```plaintext
# 忽略所有 .log 文件
*.log

# 忽略 build/ 目录
build/

# 忽略所有目录下的 temp 文件夹
**/temp/

# 不忽略 important.log
!important.log

# 忽略 .env 文件
.env
```

### 6. 注意事项
- `.gitignore` 只对未跟踪的文件有效，已跟踪的文件需手动移除：`git rm --cached <file>`。
- 每个 Git 仓库可以有多个 `.gitignore` 文件，子目录的规则会覆盖上级目录的规则。

### 7. 全局忽略
- 可设置全局忽略规则，适用于所有仓库：
  ```bash
  git config --global core.excludesfile ~/.gitignore_global
  ```

### 8. 特殊文件
- 即使被忽略，仍可使用 `git add -f <file>` 强制添加文件。

### 9. 模板
- GitHub 提供了常用语言的 `.gitignore` 模板，可在 [github/gitignore](https://github.com/github/gitignore) 查看。

通过这些规则，你可以有效管理 Git 仓库中需要忽略的文件和目录。
