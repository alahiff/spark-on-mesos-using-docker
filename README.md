# Spark on Mesos
Example running Spark on Mesos in cluster mode using Docker containers. Spark binaries don't need to be pre-installed on any nodes in the cluster.

The image used was taken from https://gitlab.cern.ch/benoel/spark-on-mesos, but updated to use Spark 2.1.0 and a correction made to ensure that cluster mode works correctly.

Run the MesosClusterDispatcher:
```
docker run --rm -it --net=host -v /opt/spark.conf:/etc/spark.conf:ro \
  alahiff/spark-on-mesos:2.1.0-3 spark-class org.apache.spark.deploy.mesos.MesosClusterDispatcher \
  --master mesos://zk://<mesos master>:2181/mesos \
  --name spark \
  --properties-file /etc/spark.conf
  ```
where here the image name, Mesos master and `spark.conf` location should be replaced as approprate. The contents of `spark.conf` also need to checked: if framework authentication is enabled the principal and secret should be specifed (or the lines removed if framework authentication is not used), a role should be specified if needed and the image specified.

A UI should be visible on port 8081 of host where the above command was run. Example launching a Spark job:
```
spark-submit --deploy-mode cluster --master mesos://hostname:7077 \
             --conf spark.mesos.executor.docker.image=alahiff/spark-on-mesos:2.1.0-3 \
             --conf spark.mesos.coarse=true \
             --conf spark.cores.max=8 \
             --conf spark.mesos.principal=<principal> \
             --conf spark.mesos.secret=<secret> \
             --conf spark.mesos.role=<role> \
             --class org.apache.spark.examples.SparkPi https://downloads.mesosphere.com/spark/assets/spark-examples_2.10-1.4.0-SNAPSHOT.jar 30
```
where here the principal and secret need to be specified (or removed), a role specified if necessary (or removed) and `hostname` should be replaced by the name of the host where the MesosClusterDispatcher is running.
