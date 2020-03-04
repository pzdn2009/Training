# 流程控制

测试成功时，表达式的退出状态为0。

```text
if [test-expr] ; then 
  …
elif [test-expr]; then
  …
else
  …
fi
```

```text
for variable in 
do
done

while
do
done

while :
do
done

for( (;;))

until
do
done
```

```text
case value in
p1)
  ;;
p2)
  ;;
*)
  ;;
esac
```

