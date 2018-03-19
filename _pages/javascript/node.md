# npm pack

将本地代码打包并生成压缩包。

# npm install

如果没有指定参数的话，安装 package.json 里设置的依赖包。也可以安装具体某个本地包，本地包可以是：

* 本地目录
* 本地 tar.gz
* url

比如

```bash
npm install /Users/gavin/github/vizceral/vizceral-react/vizceral-react-4.5.3.tgz
```

会在 package.json 中生成一条新的 dependency:

```properties
"vizceral-react": "file:../../../../../github/vizceral/vizceral-react/vizceral-react-4.5.3.tgz",
```

通过安装本地包，大大方便了本地调试。