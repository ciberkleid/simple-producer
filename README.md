# simple-producer

This project is part of a simple demonstration of the mechanics of [Spring Cloud Contract](https://spring.io/projects/spring-cloud-contract).

Setup:
```bash
git clone https://github.com/ciberkleid/simple-producer.git
PRODUCER_APP=$PWD/simple-producer
git clone https://github.com/ciberkleid/simple-consumer.git
CONSUMER_APP=$PWD/simple-consumer
```

Step 1: Producer accepts consumer-driven contract v1 and implements enough for auto-generated contract tests to pass.
```bash
cd $PRODUCER_APP
git checkout tags/v1.0.0
./mvnw clean test
```

Step 2: Consumer implements client-side of contract v1 using stub provided by producer for testing.
```bash
cd $CONSUMER_APP
git checkout tags/v1.0
./mvnw clean test
```

Step 3: Producer accepts consumer-driven contract v2 and implements enough for auto-generated contreact tests to pass.
In addition, producer ensures contract tests for v1 still pass using v2 code, thereby ensuring updated app is back-compatible with API v1.
```bash
cd $PRODUCER_APP
git checkout tags/v2.0.0
./mvnw clean test
./mvnw clean test -Papicompatibility -Dprod.version=1.0.0
```

Step 4: Consumer implements client-side of contract v2 using stub provided by producer for testing.
In addition, consumer may also choose to make updated code back-compatible with API v1
```bash
cd $CONSUMER_APP
git checkout tags/v1.1
./mvnw clean test -Dstubrunner.ids=com.example:simple-producer:2.0.0:stubs:10000
./mvnw clean test -Dstubrunner.ids=com.example:simple-producer:1.0.0:stubs:10000
```
