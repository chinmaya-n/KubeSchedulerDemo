# Demo for scheduler in Kubernetes

* Update the `hostPath` value in *volume_hello_logs.yaml* to your needs.
* Create volume & volume claim first to have persistent storage (for logs).
* Next create the cron job and verify if the job is created. Then tail the log for output.

## Important commands

``` shell
$ kubectl apply -f volume_hello_logs.yaml
persistentvolume "hello-logs-volume" created

$ kubectl apply -f volume_claim_hello_logs.yaml
persistentvolumeclaim "hello-logs-volume-claim" created

$ kubectl get pv hello-logs-volume
NAME                CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM                             STORAGECLASS   REASON    AGE
hello-logs-volume   1Gi        RWO            Retain           Bound     default/hello-logs-volume-claim   manual                   33s

$ kubectl get pvc hello-logs-volume-claim
NAME                      STATUS    VOLUME              CAPACITY   ACCESS MODES   STORAGECLASS   AGE
hello-logs-volume-claim   Bound     hello-logs-volume   1Gi        RWO            manual         42s

$ kubectl apply -f cron_hello_every_1_min.yaml
cronjob.batch "hello-date" created

$ kubectl get cronjob
NAME         SCHEDULE      SUSPEND   ACTIVE    LAST SCHEDULE   AGE
hello-date   */1 * * * *   False     0         <none>          17s

$ kubectl get jobs
NAME                    DESIRED   SUCCESSFUL   AGE
hello-date-1554321540   1         1            39s
```

## Sources

* [Volume & Claim](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)
* [Cron Job](https://medium.com/jobteaser-dev-team/kubernetes-cronjob-101-56f0a8ea7ca2)
* [Cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
