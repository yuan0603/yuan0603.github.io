## 4.0.0-beta-2-SNAPSHOT

```bash
git clone https://github.com/apache/hive.git

cd hive
```

### build

```bash
mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Pdist -Drat.skip=true 2>&1 | tee hive-build.log
```

```bash
mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Pdist -Drat.skip=true -Dprotobuf.version=3.25.1 2>&1 | tee hive-build.log
```

