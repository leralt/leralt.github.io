git reset \<commit id> 可以重置修改至这次commit，修改会保存。

### lfs
库或者其他大文件提交前，可以用git lfs track
```
git lfs track qnx_aarch64/hpc/lib/libti/*.so
```
这样会自动添加
```
linux_aarch64/hpc/lib/libti/libti_rpmsg_char.so filter=lfs diff=lfs merge=lfs -text
```
至.gitattributes
这时push的时候，就会将库正确提交

或者直接在.gitattributes里面加上`qnx_aarch64/hpc/lib/libti/*.so`也可以


如果最近的提交是你的，可以合并到上次提交
git commit --am