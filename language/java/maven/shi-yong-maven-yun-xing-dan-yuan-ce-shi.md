# 使用MAVEN运行单元测试

```text
mvn test
mvn -Dtest=TestApp1
mvn -Dtest=TestApp2

# 跳過單元測試，也跳过测试代码的编译。
mvn package -Dmaven.test.skip=true
```

