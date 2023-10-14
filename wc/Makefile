clean:
	mvn clean

install:
	mvn install

build:
	mvn package

run:
	./../../../Downloads/flink-1.16.2/bin/flink run target/wc-1.0-SNAPSHOT.jar --input /Users/noah/dev/flink-udemy/wc/wc.txt --output /Users/noah/dev/flink-udemy/wc/out.txt

start:
	/Users/noah/Downloads/flink-1.16.2/bin/start-cluster.sh
stop:
	/Users/noah/Downloads/flink-1.16.2/bin/stop-cluster.sh