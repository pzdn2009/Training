# 使用MAVEN运行单元测试

```
mvn test
mvn -Dtest=TestApp1
mvn -Dtest=TestApp2

# 跳過單元測試
mvn package -Dmaven.test.skip=true
```