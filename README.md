# Spark on Mesos
Example running Spark on Mesos using Docker containers

Run the Mesos cluster dispatcher:
```
docker run --rm -it --net=host -v /opt/spark.conf:/etc/spark.conf:ro \
  <image name> spark-class org.apache.spark.deploy.mesos.MesosClusterDispatcher \
  --master mesos://zk://<mesos master>:2181/mesos \
  --name spark \
  --properties-file /etc/spark.conf
  ```
