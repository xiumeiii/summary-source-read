[Git教程--廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

[Github 简明教程](http://www.runoob.com/w3cnote/git-guide.html)

[git-简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)

### git命令

- 分支拉取分支提交（分支为：dev）

```javascript
拉取：
git clone -b dev url
然后，进入项目的主目录里面(注：是package.json目录，千万不要在项目外面的目录下，已经跳进去2次坑了。)
然后就可操作git add  .等提交了 ....
提交：
git add .
git commit -m “提交说明"
git pull origin dev
git push origin dev 如果提交不上用这个（git push -f origin dev）
```

- 从已经clone下来的代码中，获取git的url地址

```javascript
git remote -v
```

- 关联远程仓库:git remote add origin https://***.git

```javascript
git remote -v  查看关联的远程地址
git branch  查看分支
git branch  test 切换到test分支
$ git remote   //列出已经存在的远程分支,一般返回origin
```

- git 回退到某个版本

```javascript
git log 或者 git log --pretty=oneline     (查看历史的commit记录)
git reset --hard commitId (回退到commitId这个版本)

如果要撤销commit内容--前提是在push之前，可以利用此操作
```

- git 拉取dev的以前的某个commitId版本为新分支dev_new，然后在新分支上添加功能

```javascript
1. 新建个文件夹，重新拉取dev代码 （git clone -b dev url）
2. git checkout -b dev_new （创建dev_new分支，然后切换到dev_new分支）或者 （分2步：1. git branch dev_new、2. git checkout dev_new）
3. 这时候查看分支情况：git branch （dev 和 *dev_new 2个分支，并且*号代表当前操作的分支），这时候在dev_new分支上就可以操作了
4. 查看dev_new分支上的git log --pretty=oneline 提交记录（和dev的一样）
5. 回退dev_new分支到你需要的commitId的版本，git reset --hard commitId
6. 然后随便修改个文件，git add .、git commit -m "新建dev_new分支"、git push origin dev_new
7. 完成
```


- 打tag 标签

```javascript
git tag v1.0.0 
git push origin v1.0.0

如果中途标签打错，可删除已打tag
  如果已提交到远程：git push origin --delete tag 标签名(删除远程标签) 
  如果还未提交到远程：git tag -d 标签名(删除本地标签))
然后再重新打tag标签

$ git tag (查看已有的tag标签)
```



- 合并分支到主线

  ```javascript
  git checkout master

  git merge dev(可直接进行git push origin master)

  git commit(会报nothing to commit,working tree clean)

  git status(查看冲突，如果没问题后直接push)

  git push origin master
  ```

- git 命令遇到的一些问题
1. git clone出现fatal: unable to access 'https://': SSL certificate problem: self signed certificate in c
> git config --global http.sslVerify false 再不行 执行：env GIT_SSL_NO_VERIFY=true git clone url(env:是一个外部命令，程序文件/bin/env，列出所有环境变量及其赋值。)

### github的使用技巧



*  trending

   ```javascript
    Explore 菜单 --- 点击 Trending 按钮  可以选择「当天热门」、「一周之内热门」和「一月之内热门」来查看
   ```

*  search

   ```javascript
   比如搜索js相关  输入 js stars:>50000 即可搜索出stars大于50k的项目
   ```

   ​

*  福利大放送

   *  >[free-programming-books](https://github.com/vhf/free-programming-books): 这个项目整理了所有跟编程相关的**免费书籍**，而且全球多国语言版的都有，中文版的在这里：[free-programming-books-zh](https://github.com/vhf/free-programming-books/blob/master/free-programming-books-zh.md)
      >
      >[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) : 不会用 shell 的程序员不是真正的程序员，所以建议每个程序员都懂点 shell
      >
      >[awesome](https://github.com/sindresorhus/awesome) : GitHub 上有各种 awesome 系列，简单来说就是这个系列搜罗整理了 GitHub 上各领域的资源大汇总，比如有 awesome-android, awesome-ios, awesome-java, awesome-python 等等等，就不截图了，你们自行去感受。
      >
      >- [github-cheat-sheet](https://github.com/tiimgreen/github-cheat-sheet/)
      >
      >GitHub 的使用有各种技巧，只不过基本的就够我们用了，但是如果你对 GitHub 超级感兴趣，想更多的了解 GitHub 的使用技巧，那么这个项目就刚好是你需要的，每个 GitHub 粉都应该知道这个项目。
      >
      >- [android-open-project](https://github.com/Trinea/android-open-project)
      >
      >这个项目是我一个好朋友 Trinea 整理的一个开源项目，基本囊括了所有 GitHub 上的 Android 优秀开源项目，但是缺点就是太多了不适合快速搜索定位，但是身为 Android 开发无论如何你们应该知道这个项目。
      >
      >- [awesome-android-ui](https://github.com/wasabeef/awesome-android-ui)
      >
      >这个项目跟上面的区别是，这个项目只整理了所有跟 Android UI 相关的优秀开源项目，基本你在实际开发终于到的各种效果上面都几乎能找到类似的项目，简直是开发必备。
      >
      >- [LearningNotes](https://github.com/GeniusVJR/LearningNotes)
      >
      >这是一份非常详细的面试资料，涉及 Android、Java、设计模式、算法等等等，你能想到的，你不能想到的基本都包含了，可以说是适应于任何准备面试的 Android 开发者，看完这个之后别说你还不知道怎么面试！


---

[git 搜索技巧-语法](https://help.github.com/articles/understanding-the-search-syntax/)
[github代码搜索技巧](http://blog.sina.com.cn/s/blog_4e60b09d0102vcso.html)

https://github.com/MMF-FE/vue-svgicon