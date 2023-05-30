#### 常见java启动、停止、监控脚本
##### 启动脚本
start.sh:
```sql
#!/bin/bash
echo  "current path is ： $(pwd)"
source  /etc/profile

# stop
sh /xx/xx/stop.sh

# 设置JAVA_HOME和CLASSPATH
export JAVA_HOME=/xx/xx/java
export BASE_DIR=/xx/xx/x

# 设置服务名称
SERVICE_NAME=your_service_namexx

JAVA_OPTS="${JAVA_OPTS} -server -Xms4g -Xmx4g -Xmn2048m"
JAVA_OPTS="${JAVA_OPTS} -XX:+UseG1GC -XX:G1HeapRegionSize=32m -XX:G1ReservePercent=25 -XX:InitiatingHeapOccupancyPercent=30 -XX:SoftRefLRUPolicyMSPerMB=0"
JAVA_OPTS="${JAVA_OPTS} -verbose:gc -Xloggc:${BASE_PATH}/gclogs/gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintGCApplicationStoppedTime -XX:+PrintAdaptiveSizePolicy"
JAVA_OPTS="${JAVA_OPTS} -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=5 -XX:GCLogFileSize=100m"
JAVA_OPTS="${JAVA_OPTS} -XX:-OmitStackTraceInFastThrow"
JAVA_OPTS="${JAVA_OPTS} -XX:+AlwaysPreTouch"
JAVA_OPTS="${JAVA_OPTS} -XX:-UseLargePages -XX:-UseBiasedLocking"

# 启动服务
echo "start $SERVICE_NAME ..."
nohup $JAVA_HOME/bin/java -jar $JAVA_OPTS -Dserver.port=xx /BASE_DIR/xx.jar > /dev/null 2>&1 &

echo "Service $SERVICE_NAME is starting..."
```

##### 停止脚本
stop.sh
```sql
#!/bin/bash

# 设置JAVA_HOME和CLASSPATH
export JAVA_HOME=/path/to/java
export CLASSPATH=/path/to/classpath

# 设置服务名称
SERVICE_NAME=your_service_name

# 停止服务
PID=  ps -ef | grep $SERVICE_NAME | grep -v grep | awk '{print $2}'  
if [ -n "$PID" ]; then
kill $PID
echo "Service $SERVICE_NAME is stopping..."
else
echo "Service $SERVICE_NAME is not running."
fi
```
##### 监控脚本
monitor.sh
```sql
#!/bin/bash

# 设置JAVA_HOME和CLASSPATH
export JAVA_HOME=/path/to/java
export CLASSPATH=/path/to/classpath

# 设置服务名称
SERVICE_NAME=your_service_name

# 检查服务是否运行
PID=  ps -ef | grep $SERVICE_NAME | grep -v grep | awk '{print $2}'  
if [ -n "$PID" ]; then
  echo "Service $SERVICE_NAME is running with PID $PID."
else
  echo "Service $SERVICE_NAME is not running."
fi
```