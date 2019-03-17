# Java 自带命令使用

## jar

```shell
jar {ctxu}[vfm0M] [jar-文件] [manifest-文件] [-C 目录] 文件名 …
```
其中 `{ctxu}` 是 jar 命令的子命令，每次 jar 命令只能包含 ctxu 中的一个，它们分别表示：

    -c　创建新的 JAR 文件包
    -t　列出 JAR 文件包的内容列表
    -x　展开 JAR 文件包的指定文件或者所有文件

    -u　更新已存在的 JAR 文件包 (添加文件到 JAR 文件包中)
    [vfm0M] 中的选项可以任选，也可以不选，它们是 jar 命令的选项参数
    -v　生成详细报告并打印到标准输出
    -f　指定 JAR 文件名，通常这个参数是必须的
    -m　指定需要包含的 MANIFEST 清单文件
    -0　只存储，不压缩，这样产生的 JAR 文件包会比不用该参数产生的体积大，但速度更快
    -M　不产生所有项的清单（MANIFEST〕文件，此参数会忽略 -m 参数

```shell
#创建jar包并显示打包过程
jar -cvf filename.jar files

#创建可执行jar包并显示打包过程
jar -cvfm filename.jar MANIFEST.MF files

#查看jar包中的文件
jar -tf filename.jar

#解压jar包并显示打包过程
jar -xvf filename.jar

#向jar包中添加文件
jar -uf filename.jar files

#(加-C参数，表示先切换到TEST目录下在执行jar -cvf命令)
JAR -CVF FILENAME.JAR -C TEST/
```
