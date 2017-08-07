# cat重定向写入

将stdin的内容重定向到test文件(以覆盖文件内容的方式，若此文件不存在，则创建之)，且当stdin中含有EOF时完成写入：
ufo@ufo:/tmp$ cat > test << EOF
> this is first line 
> this is second line
> this is thrird line
> this is fourth line
> EOF

ufo@ufo:/tmp$ cat test 
this is first line 
this is second line
this is thrird line
this is fourth line

将stdin的内容重定向以添加的方式到已经存在test文件，且当stdin中含有EOF时完成写入：
ufo@ufo:/tmp$ cat >> test < EOF
bash: EOF: 没有那个文件或目录
ufo@ufo:/tmp$ cat >> test << EOF
> this is five line
> this is sixth line
> EOF
ufo@ufo:/tmp$ cat test 
this is first line 
this is second line
this is thrird line
this is fourth line
this is five line
this is sixth line

说明：以上的EOF可以为你想要的其它字符串。