## 0.10.3-SNAPSHOT

```bash
git clone https://github.com/apache/tez.git

cd tez
```

* tez-ui/src/main/webapp/app/routes/server-side-ops.js

```bash
# line 77: that.get("loadedValue").pushObjects(data.content);
that.get("loadedValue").pushObjects(data);
```

* tez-ui/src/main/webapp/app/utils/table-definition.js

```bash
# line: rowCountOptions: [5, 10, 25, 50, 100],
rowCountOptions: [5, 10, 15, 20, 25, 50, 100],
```

### build

```bash
mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Phadoop28 -P\!hadoop27
```

```bash
mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Phadoop28 -P\!hadoop27 -Dprotobuf.version=3.25.1
```

```bash
mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Phadoop28 -P\!hadoop27 -Dhadoop.version=3.3.6
```

```bash
mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Phadoop28 -P\!hadoop27 -Dprotoc.path=/usr/bin/protoc
```

```bash
mvn clean package -DskipTests -Dmaven.javadoc.skip=true -Phadoop28 -P\!hadoop27 2>&1 | tee tez-build.log
```
