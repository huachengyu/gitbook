### JAR包冲突解决办法
@Date 2017.05.24

> maven

```
# 打印所有MVN依赖
mvn dependency:tree
```

```
# 打印指定groupId和artifactId的依赖关系
mvn dependency:tree -Dverbose -Dincludes=groupId:artifactId
```

