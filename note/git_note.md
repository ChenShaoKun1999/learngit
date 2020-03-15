# 基本概念

## 工作区，暂存区和版本库

工作区(working directory)就是我们看见的目录，暂存区(index, or stage)通常在.git/index文件夹，版本库(repository)就是.git文件夹下的其他东西，包括了所有历史版本。程序员在工作区编辑，通过add把**修改**添加进暂存区，再通过commit提交到版本库

工作流程

# 常用指令

```
init     初始化，建立.git文件夹

add <filename> 给项目添加文件，或者把修改加入暂存区
rm <filename>  删除文件
mv oldpath newpath 重命名或移动
commit         提交项目，需要先用add把修改放进暂存区stage
    -m "message"
    -a            提交全部文件
checkout -- <filename>  恢复到上一次add或commit的状态

查看状态
status    状态（哪些文件被修改）
diff      当前与上次commit的区别（有具体文本）
log       历史版本的log

与远程仓库同步例子
remote add origin git@github.com:michaelliao/learngit.git   添加远程库
clone git@github.com:michaelliao/gitskills.git   克隆仓库
```

