# 1.git config 配置

初始化使用时需要对git进行配置

初始化名称

```powershell
git config --global user.name "Your GitHub name"
```

初始化邮箱p

```powershell
git config --global user.email "your Github email"
```

可以使用该命令对姓名和邮箱进行保存，往后不用每次都输入了

```powershell
git config --global credential.helper store
```

查看当前的配置信息

```powershell
git config --global --list
```

# 2.新建仓库

## 1.新建仓库

```powershell
git init
```

此时会生成隐藏文件.git

# 3.工作区域和文件状态

## 1.工作区域

工作区：自己电脑上的目录

暂存区：用于保存即将提交到仓库的内容

本地仓库：是git init命令新建的仓库，是Git存储代码和版本信息的主要位置

![image-20240730151352915](.\images\image-20240730151352915.png)

## 2.文件状态

未跟踪：创建的新文件还未被Git管理

未修改：已被Git管理但还未修改

已修改：已被Git管理且已修改

已暂存：已经放入暂存区的文件

![image-20240730151824208](.\images\image-20240730151824208.png)

# 4.添加和提交文件

## 1.查看git的状态

可以查看当前文件夹中各文件的状态

```powershell
git status
```

## 2.将工作区中的文件提交到暂存区

可以使用linux中的通配符

```powershell
git add 文件名
git add .  //可以提交文件夹中所有内容
```

查看暂存区的文件

```
git ls-files
```



## 3.将暂存区的文件提交到本地仓库

```powershell
git commit -m "提交文件的说明"
```

不输入 -m 参数会进入vim编辑器，可在vim编辑器中输入提交文件的说明

## 4.查看提交记录

```
git log
git log --oneline 显示简介的提交信息
```

# 5.git reset回退版本

## 1.soft

版本id在git log 中查看

```powershell
git reset --soft 回退的版本id
例：git reset --soft 5ef70b8
```

回退到上一个版本，保存工作区和暂存区的修改内容

## 2.hard

```powershell
git reset --hard 回退的版本id
```

回退到上一个版本，丢弃工作区和暂存区的修改内容

## 3.mixed

mixed是reset命令的默认参数

```powershell
git reset --mixed 回退的版本id  
```

回退到上一个版本，保存工作区的修改内容，丢弃暂存区的修改内容

## 4.回溯操作

git中所有操作都是可回溯的

```
git reglog //查看操作记录
git reset --hard 操作id
```

