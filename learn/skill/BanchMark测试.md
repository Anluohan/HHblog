[Windows测试框架搭建参考链接](https://gitee.com/openharmony/test_developertest#windows%E7%8E%AF%E5%A2%83%E6%89%A7%E8%A1%8C).

1.找到.\Test\developertest目录下的start.bat运行

2.run -t [测试类型] -ts [测试套名字]，例如
`run -t BENCHMARK -ts BenchmarkTestForDistributedBundleInfo`

3.从设备中找到奔溃报告（也有可能没有发生奔溃，那就没有报告），将其复制到windows文件夹中查看。可使用如下命令复制.
`hdc_std shell file recv /data/log/faultlog/ C:\Users\zhuhan\Desktop\log\faultlog`
可能会出现`permission denied`报错。此时在设备上使用`cat`命令将该日志内容复制出来。

4.定位问题





