Yarn 3.1.1 에서
spark application을 yarn-cluster mode로 실행 했을 때
랜덤하게 Yarn에서는 app이 정상 종료 되었다고 나오지만
spark history server에서는 영원히 incomplete로 남아있는 경우가 생김

이럴 경우 cluster resource 가 충분한 상황인데도
job이 영원히 pending되는 상황이 발생하는데
증상을 보면 AM이 동작하기 위한 resource가 부족하다고 나옴
하지만 AM이 사용할수 있는 resource도 충분한 상황임

그런데
http://namenode:8088/ws/v1/cluster/schedulers
API를 사용해 스케쥴러 상태를 확인해 보면
amused가 spark incomplete app만큼 더 사용하고 있는걸로
보이는걸 확인 할 수 있음

spark incomplete app이 AM resource를 사용하고 있는걸로 되어서
job이 pending되는걸로 보임



https://stackoverflow.com/questions/49180993/spark-job-completes-but-shows-incomplete-tasks-spark
https://stackoverflow.com/questions/52126052/what-is-active-jobs-in-spark-history-server-spark-ui-jobs-section


해결법 1. https://community.cloudera.com/t5/Support-Questions/Spark-UI-thinks-job-is-incomplete/td-p/116071/page/2
해결법 2. code에서 sc.stop() 명시적으로 call


