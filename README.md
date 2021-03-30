http://log4jdbc.brunorozendo.com/

`twitter-text` 라이브러리를 사용하여 실제로 보이는 문자의 칸을 계산해서 조금 더 이쁘게 로그가 출력되도록 했다. 

영문자가 아닌 다른 글자가 섞여도 테이블의 킨이 제대로 맞춰져서 나온다.

수정 전(Before):

```
|--------|------------------------|----------|-----------|--------|------------------------|------------|
|mmbr_id |rgst_dt                 |mmbr_hndc |kko_lgn_id |mgnr_yn |mdfy_dt                 |mmbr_nck_nm |
|--------|------------------------|----------|-----------|--------|------------------------|------------|
|2       |2021-03-31 02:12:20.789 |20        |2          |N       |2021-03-31 02:12:20.789 |띠용          |
|--------|------------------------|----------|-----------|--------|------------------------|------------|
```

수정 후(After):

```
|--------|------------------------|----------|-----------|--------|------------------------|------------|
|mmbr_id |rgst_dt                 |mmbr_hndc |kko_lgn_id |mgnr_yn |mdfy_dt                 |mmbr_nck_nm |
|--------|------------------------|----------|-----------|--------|------------------------|------------|
|2       |2021-03-31 02:10:27.868 |20        |2          |N       |2021-03-31 02:10:27.868 |띠용        |
|--------|------------------------|----------|-----------|--------|------------------------|------------|
```

메이븐 설정 예:

```xml
    <repositories>
        <repository>
            <id>jfrog</id>
            <url>https://antop.jfrog.io/artifactory/maven/</url>
        </repository>
    </repositories>

    <dependencies>
        <dependency>
            <groupId>org.bgee.log4jdbc-log4j2</groupId>
            <artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>
            <version>1.16-kr</version>
        </dependency>
    </dependencies>
```

그래들 설정 예:

```kotlin
repositories {
    mavenCentral()
    maven {
        url = uri("https://antop.jfrog.io/artifactory/maven/")
        metadataSources {
            mavenPom()
            artifact()
        }
    }
}

dependencies {
    // https://github.com/candrews/log4jdbc-spring-boot-starter/
    implementation("com.integralblue:log4jdbc-spring-boot-starter:2.0.0") {
        // exclude 1.16
        exclude group: "org.bgee.log4jdbc-log4j2", module: "log4jdbc-log4j2-jdbc4.1"
    }
    implementation "org.bgee.log4jdbc-log4j2:log4jdbc-log4j2-jdbc4.1:1.16-kr"
}
```
