 ## fg、bg
 ctrl z stop的进程可以通过fg <job号> 恢复到前台， bg <job号> 恢复到后台
 ## ln
 更换软连接指向 ln -snf <新的指向> <软连接文件>
 ## du
 查看当前目录下的文件夹包含内容大小
 ```bash
 du -lh --max-depth=1
```
## ldd
查看依赖库

## perf
https://github.com/brendangregg/FlameGraph
Using Linux perf_events (aka "perf") to capture 60 seconds of 99 Hertz stack samples, both user- and kernel-level stacks, all processes:

```bash
perf record -F 99 -a -g -- sleep 60
perf script > out.perf
```

Now only capturing PID 181:

```bash
perf record -F 99 -p 181 -g -- sleep 60
perf script > out.perf
```
