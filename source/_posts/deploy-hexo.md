---
title: 通过Github Actions部署你的Hexo博客
---
最近被生活中的琐事所困扰，实在有些迷茫，因此又双叒叕的骚动了想要记录点事情的冲动，思来想去，出于对nodejs相关技术栈的亲切感，
选了hexo。

## 使用hexo d进行部署

1. 增加 `hexo-deployer-git` 依赖，在_config.yaml文件里配置 
```
deploy:
  - type: git #提交类型git
    repo: git@github.com:xxx/xxx.github.io.git
    branch: master
```

2. 增加部署命令：` "deploy": "hexo clean && hexo g -d"`,执行 `npm run deploy`,然后打开你的github page即可看到你的博客啦。

这些其实官网中都有相关的介绍了，但是有个问题，打开你的github仓库能够看到，仓库中的代码实际上是编译后的静态js html css文件，这就导致你需要一个单独的仓库去管理你的源码。

## 使用github action进行部署

虽然其实严格来说上述的步骤也不麻烦，只是需要每次执行push前做一次npm run deploy，但是在阅读文档时，看到介绍，可以采用github action来做部署，因此果断尝试一下

1. 执行 ` ssh-keygen -t rsa -C "email" `,不要默认命名，输入类似  xxx_blog_deploy  的名字方便你后续找到对应的 ssh key
2. 在你的github page仓库的Settings/Deploy keys 增加xxx_blog_deploy.pub的内容，并注意要基于write access
3. 在你的blog source仓库的Settings/Secrets 仓库增加xxx_blog_deploy 的内容
4. 在你的source的.github/workflows 文件夹下增加 xxx.yml (我用的pnpm, npm的缓存方式可以参考`sma11black/hexo-action`示例)
```
name: Deploy

on:
  - push
  - pull_request

jobs:
  build:
    runs-on: ubuntu-latest
    name: A job to deploy hexo blog.
    strategy:
      matrix:
        node: [ '12', '14', '16' ]
    steps:
    - name: Checkout
      uses: actions/checkout@v2
      with:
        submodules: true # Checkout private submodules(themes or something else).
    - name: Setup node
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node }}
    - name: Cache pnpm modules
      uses: actions/cache@v2
      with:
        path: ~/.pnpm-store
        key: ${{ runner.os }}-${{ hashFiles('**/pnpm-lock.yaml') }}
        restore-keys: |
          ${{ runner.os }}-
    - uses: pnpm/action-setup@v2.0.1
      with:
        version: 6.0.2
        run_install: true
    
    # Deploy hexo blog website.
    - name: Deploy
      id: deploy
      uses: sma11black/hexo-action@v1.0.3
      with:
        deploy_key: ${{ secrets.DEPLOY_KEY }}
        user_name: xxx  # (or delete this input setting to use bot account)
        user_email: xxx@xxx.com  # (or delete this input setting to use bot account)
        commit_msg: ${{ github.event.head_commit.message }}  # (or delete this input setting to use hexo default settings)
    # Use the output from the `deploy` step(use for test action)
    - name: Get the output
      run: |
        echo "${{ steps.deploy.outputs.notify }}"
```
5. 每次git push后会自动触发github action 帮你生成静态文件，并且将静态文件推到 github page仓库了。


希望这次是个好的开始，希望能坚持输出博客~