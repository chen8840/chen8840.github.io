### NPM cache相关

今天下午把package.lock.json用别人的替换了，然后编译一堆报错，这个问题弄了一下午。

总结一下经验：

1.关于npm cache

    NPM会把所有下载的包保存，放在用户文件夹下面，在我的windows10机器上是保存在C:\Users\zcche\AppData\Roaming\npm-cache下面

2.关于package.lock.json

    NPM install之后会计算每个包的sha1值，然后将包与他的sha1值关联保存在package.lock.json里面

    下次NPM install的时候会根据package.lock.json里面保存的sha1值去文件夹C:\Users\zcche\AppData\Roaming\npm-cache里面寻找包文件，如果存在，就不用再次从网上下载安装报了

3.NPM cache verify

    目测这个命令是重新计算C:\Users\zcche\AppData\Roaming\npm-cache下的文件是否与sha1值匹配，如果不匹配可能删除？

4.NPM cache clean --force

    这个命令从C:\Users\zcche\AppData\Roaming\npm-cache下删除所有缓存文件



坑：

    NPM不同版本算出来的sha1貌似不完全一样，所以直接用别人的package.lock.json会报sha1不匹配的error

解决办法：

    1.不使用别人的package.lock.json

    2.如果用了，删掉package.lock.json(记得删除回收站里的)，npm cache clear --force，npm install
