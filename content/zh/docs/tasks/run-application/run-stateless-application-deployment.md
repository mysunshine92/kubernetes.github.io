---
title: 使用  Deployment 运行无状态的应用
min-kubernetes-server-version: v1.9
content_template: templates/tutorial
weight: 10
---

{{% capture overview %}}

<!--
This page shows how to run an application using a Kubernetes Deployment object.
--->
本文展示了如何使用 Kubernetes 的 Deployment 对象运行一个应用。

{{% /capture %}}


{{% capture objectives %}}

<!--
* Create an nginx deployment.
* Use kubectl to list information about the deployment.
* Update the deployment.
--->
* 创建一个 nginx deployment。
* 使用 kubectl 列出有关 deployment 的信息。
* 更新 deployment。

{{% /capture %}}


{{% capture prerequisites %}}

{{< include "task-tutorial-prereqs.md" >}} {{< version-check >}}

{{% /capture %}}


{{% capture lessoncontent %}}

<!--
## Creating and exploring an nginx deployment
--->
## 创建并且研究 nginx deployment

<!--
You can run an application by creating a Kubernetes Deployment object, and you
can describe a Deployment in a YAML file. For example, this YAML file describes
a Deployment that runs the nginx:1.7.9 Docker image:
--->
您可以通过创建 Kubernetes 的 Deployment 对象来运行一个应用，并且您可以在 YAML 文件中描述 Deployment。比如，下面这个 YAML 文件描述了使用 nginx:1.7.9 镜像的 Deployment。

{{< codenew file="application/deployment.yaml" >}}


<!--
1. Create a Deployment based on the YAML file:
--->
1. 基于 YAML 文件创建一个 Deployment：

        kubectl apply -f https://k8s.io/examples/application/deployment.yaml

<!--
2. Display information about the Deployment:
--->
2. 显示有关 Deployment 的信息：

        kubectl describe deployment nginx-deployment

    The output is similar to this:

        user@computer:~/website$ kubectl describe deployment nginx-deployment
        Name:     nginx-deployment
        Namespace:    default
        CreationTimestamp:  Tue, 30 Aug 2016 18:11:37 -0700
        Labels:     app=nginx
        Annotations:    deployment.kubernetes.io/revision=1
        Selector:   app=nginx
        Replicas:   2 desired | 2 updated | 2 total | 2 available | 0 unavailable
        StrategyType:   RollingUpdate
        MinReadySeconds:  0
        RollingUpdateStrategy:  1 max unavailable, 1 max surge
        Pod Template:
          Labels:       app=nginx
          Containers:
           nginx:
            Image:              nginx:1.7.9
            Port:               80/TCP
            Environment:        <none>
            Mounts:             <none>
          Volumes:              <none>
        Conditions:
          Type          Status  Reason
          ----          ------  ------
          Available     True    MinimumReplicasAvailable
          Progressing   True    NewReplicaSetAvailable
        OldReplicaSets:   <none>
        NewReplicaSet:    nginx-deployment-1771418926 (2/2 replicas created)
        No events.

<!--
3. List the pods created by the deployment:
--->
3. 列出deployment创建的 pods: 

        kubectl get pods -l app=nginx

    The output is similar to this:

        NAME                                READY     STATUS    RESTARTS   AGE
        nginx-deployment-1771418926-7o5ns   1/1       Running   0          16h
        nginx-deployment-1771418926-r18az   1/1       Running   0          16h

<!--
4. Display information about a pod:
--->
4. 显示有关 pod 的信息：

        kubectl describe pod <pod-name>

<!--
    where `<pod-name>` is the name of one of your pods.
--->
    其中 `<pod-name>` 是其中一个 pod 的名字。

<!--
## Updating the deployment
--->
## 更新 deployment

<!--
You can update the deployment by applying a new YAML file. This YAML file
specifies that the deployment should be updated to use nginx 1.8.
--->
您可以通过使用新的 YAML 文件来更新 deployment。这个 YAML 文件指定应该更新 deployment 以使用 nginx 1.8。

{{< codenew file="application/deployment-update.yaml" >}}

<!--
1. Apply the new YAML file:
--->
1. 使用新的 YAML 文件：

         kubectl apply -f https://k8s.io/examples/application/deployment-update.yaml

<!--
2. Watch the deployment create pods with new names and delete the old pods:
--->
2. 观察 deployment 使用新名字创建了 pods 同时删除了旧的 pods:

         kubectl get pods -l app=nginx

<!--
## Scaling the application by increasing the replica count
--->
## 通过增加副本数来扩容应用

<!--
You can increase the number of pods in your Deployment by applying a new YAML
file. This YAML file sets `replicas` to 4, which specifies that the Deployment
should have four pods:
--->
通过使用新的 YAML 文件，您可以增加 Deployment 中的 pods数。这个 YAML 文件设置 `replicas` 为4，它指定了 Deployment 应该有四个 pods:

{{< codenew file="application/deployment-scale.yaml" >}}

<!--
1. Apply the new YAML file:
--->
1. 使用新的 YAML 文件：

        kubectl apply -f https://k8s.io/examples/application/deployment-scale.yaml

<!--
2. Verify that the Deployment has four pods:
--->
2. 验证 Deployment 有四个 pods:

        kubectl get pods -l app=nginx

    The output is similar to this:

        NAME                               READY     STATUS    RESTARTS   AGE
        nginx-deployment-148880595-4zdqq   1/1       Running   0          25s
        nginx-deployment-148880595-6zgi1   1/1       Running   0          25s
        nginx-deployment-148880595-fxcez   1/1       Running   0          2m
        nginx-deployment-148880595-rwovn   1/1       Running   0          2m

## 删除 deployment

<!--
Delete the deployment by name:
--->
通过名字删除deployment

    kubectl delete deployment nginx-deployment

## ReplicationControllers -- 老的方式

<!--
The preferred way to create a replicated application is to use a Deployment,
which in turn uses a ReplicaSet. Before the Deployment and ReplicaSet were
added to Kubernetes, replicated applications were configured using a
[ReplicationController](/docs/concepts/workloads/controllers/replicationcontroller/).
--->
创建多副本应用的首选方法是使用 Deployment，而 Deployment 又使用 ReplicaSet。在将 Deployment 和 ReplicaSet 添加到 Kubernetes之前，使用 [ReplicationController](/docs/concepts/workloads/controllers/replicationcontroller/) 配置多副本的应用。
{{% /capture %}}


{{% capture whatsnext %}}

<!--
* Learn more about [Deployment objects](/docs/concepts/workloads/controllers/deployment/).
--->
学习更多关于 [Deployment 对象](/docs/concepts/workloads/controllers/deployment/)。

{{% /capture %}}


