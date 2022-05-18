# K8_Assignment1

1. diff 

kind: ReplicationController

kind: ReplicaSet


metadata:
  name: kubia2

metadata:
  name: kubia


selector:

selector:
    matchLabels:



ports:
        - containerPort: 8080

No ports 


2. [root@ip-172-31-20-159 05-services]# kubectl get po -n vicky
NAME           READY   STATUS    RESTARTS   AGE
kubia-manual   1/1     Running   0          4d19h


[root@ip-172-31-20-159 05-services]# kubectl apply -f kubia-replicaset.yaml
replicaset.apps/kubia created

3 replicas cre
[root@ip-172-31-20-159 05-services]# kubectl get po -n vicky
NAME           READY   STATUS    RESTARTS   AGE
kubia-5tsb7    1/1     Running   0          5m4s
kubia-7stjx    1/1     Running   0          5m5s
kubia-manual   1/1     Running   0          4d19h
kubia-vnwdj    1/1     Running   0          5m5s


[root@ip-172-31-20-159 05-services]# kubectl get rs
NAME    DESIRED   CURRENT   READY   AGE
kubia   3         3         3       13m


[root@ip-172-31-20-159 05-services]# kubectl get po --show-labels
NAME           READY   STATUS    RESTARTS   AGE     LABELS
kubia-5tsb7    1/1     Running   0          19m     app=kubia
kubia-7stjx    1/1     Running   0          19m     app=kubia
kubia-manual   1/1     Running   0          4d20h   app=ribbon,rel=prod
kubia-vnwdj    1/1     Running   0          19m     app=kubia


kubectl get ns 


[root@ip-172-31-20-159 05-services]# vi kubia-svc.yaml
[root@ip-172-31-20-159 05-services]#  kubectl apply -f kubia-svc.yaml
service/kubia2 created
[root@ip-172-31-20-159 05-services]# kubectl get svc
NAME     TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
kubia2   ClusterIP   10.96.10.97   <none>        80/TCP    6s
[root@ip-172-31-20-159 05-services]#
  
  
  ################ cron jobs ##################################
  
  ################################## cron jobs ########################################

[root@ip-172-31-20-159 04-controllers]# ls
batch-job                                kubia-rc.yaml                           multi-completion-parallel-batch-job.yaml
batch-job.yaml                           kubia-replicaset-matchexpressions.yaml  ssd-monitor
cronjob.yaml                             kubia-replicaset.yaml                   ssd-monitor-daemonset.yaml
kubia-liveness-probe-initial-delay.yaml  kubia-unhealthy                         time-limited-batch-job.yaml
kubia-liveness-probe.yaml                multi-completion-batch-job.yaml
[root@ip-172-31-20-159 04-controllers]# vi cronjob.yaml
[root@ip-172-31-20-159 04-controllers]#
[root@ip-172-31-20-159 04-controllers]#
[root@ip-172-31-20-159 04-controllers]# kubectl apply -f cronjob.yaml
cronjob.batch/batch-job-every-fifteen-minutes created
[root@ip-172-31-20-159 04-controllers]#a
[root@ip-172-31-20-159 04-controllers]#
[root@ip-172-31-20-159 04-controllers]#
[root@ip-172-31-20-159 04-controllers]#
[root@ip-172-31-20-159 04-controllers]#
[root@ip-172-31-20-159 04-controllers]#
[root@ip-172-31-20-159 04-controllers]# kubectl describe cronjob.batch/batch-job-every-fifteen-minutes
Name:                          batch-job-every-fifteen-minutes
Namespace:                     vicky
Labels:                        <none>
Annotations:                   <none>
Schedule:                      0,15,30,45 * * * *
Concurrency Policy:            Allow
Suspend:                       False
Successful Job History Limit:  3
Failed Job History Limit:      1
Starting Deadline Seconds:     <unset>
Selector:                      <unset>
Parallelism:                   <unset>
Completions:                   <unset>
Pod Template:
  Labels:  app=periodic-batch-job
  Containers:
   main:
    Image:           luksa/batch-job
    Port:            <none>
    Host Port:       <none>
    Environment:     <none>
    Mounts:          <none>
  Volumes:           <none>
Last Schedule Time:  <unset>
Active Jobs:         <none>
Events:              <none>
[root@ip-172-31-20-159 04-controllers]#

[root@ip-172-31-20-159 04-controllers]# kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
kubia-5tsb7    1/1     Running   0          38m
kubia-7stjx    1/1     Running   0          38m
kubia-manual   1/1     Running   0          4d20h
kubia-vnwdj    1/1     Running   0          38m
[root@ip-172-31-20-159 04-controllers]# pods=$(kubectl get pods --selector=job-name=pi --output=jsonpath='{.items[*].metadata.name}')
[root@ip-172-31-20-159 04-controllers]# echo $pods^C
[root@ip-172-31-20-159 04-controllers]# pods=$(kubectl get pods --selector=job-name=batch-job-every-fifteen-minutes --output=jsonpath='{.items[*].metadata.name}')
[root@ip-172-31-20-159 04-controllers]# echo $pods

[root@ip-172-31-20-159 04-controllers]# echo $pods
