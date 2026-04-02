# K8s Pod 创建埋点处理(Mutating Admission Webhook)_pod 埋点

> 来源：[https://geekbang.feishu.cn/docx/UK7zd8TgcoHCqzxwLzhcwE7Nnwg](https://geekbang.feishu.cn/docx/UK7zd8TgcoHCqzxwLzhcwE7Nnwg)

> 👨🏻 作者：山河已无恙

> 📃 作者介绍：SRE，云原生相关技术干货分享，RHCA/RHCE/RHCAS、CKA 认证，CSDN/51CTO博客专家，InfoQ写作平台/Alibaba Cloud社区签约作者、Huawei Cloud社区云享专家、Tencent Cloud社区内容共创官

> ⏰ 收录时间：2024-05-27 11:28:10 (UTC+8)

> 🔗 原文链接： h[ttps://liruilong.b](https://liruilong.blog.csdn.net/article/details/134426489)log.csdn.net/art...

![](img/689f55c5de9a36860e40c00de46a7a53.png)

关注作者，获取更多AI知识

###   写在前面

---

+   工作中涉及到相关的知识

+   在实际的生产中，我们可以有需求对 创建的资源做一些类似 埋点 相关的操作，比如添加一些 Pod 创建时的自定义验证逻辑，类似表单提交验证那样，或者希望对创建的资源对象进行加工，在比如给资源对象添加对应的 zone 标签，涉及到 SC 相关的标签，或者根据命名空间动态织入亲和性和拓扑相关约束,添加一些 卷

+   上面的这些需求我们可以通过 k8s Admission Webhook 来完成，博文为查阅资料整理笔记，内容为涉及

+   一个大佬写好的 自定义准入控制器 Demo 学习

+   理解不足小伙伴帮忙指正

**对每**个人而言，真正的职责只有一个：找到自我。然后在心中坚守其一生，全心全意，永不停息。所有其它的路都是不完整的，是人的逃避方式，是对大众理想的懦弱回归，是随波逐流，是对内心的恐惧 ——赫尔曼·黑塞《德米安》

---

学习之前，建议看看这两篇官网的文章：

准入 [控制器 ](https://so.csdn.net/so/search?q=%E6%8E%A7%E5%88%B6%E5%99%A8&spm=1001.2101.3001.7020)参考:

[https://kubernetes.io/docs/reference/a](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)ccess-authn-authz/admission-controllers/

Kubernetes 准入控制器指南:

[https://kubernetes.io/blog/2019/03/21/a-gu](https://kubernetes.io/blog/2019/03/21/a-guide-to-kubernetes-admission-controllers/)ide-to-kubernetes-admission-controllers/

学习的项目文章：

[https://didil.medium.com/building-a-kubernetes](https://didil.medium.com/building-a-kubernetes-mutating-admission-webhook-7e48729523ed)-mutating-admission-webhook-7e48729523ed

翻译版本：

[h](https://cloudnative.to/blog/mutating-admission-webhook/)ttps://cloudnative.to/blog/mutating-admission-webhook/

通过 Admission Webhook 为每个创建的 Pod (打了指定标签) 添加一个 CM ，以卷的方式挂载，类似为 每个命名空间 中的 Pod 自动挂载当前命名默认生成 SA 的 token 一样。

克隆一下作者的项目：

[https://github.com/didil/k8s-he](https://github.com/didil/k8s-hello-mutating-webhook)llo-mutating-webhook

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook]
└─$git clone  https://github.com/didil/k8s-hello-mutating-webhook.git
```

确认一下 对应的 Webhook 钩子对应的镜像能不能使用

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/deployment]
└─$docker pull quay.io/didil/hello-webhook:0.1.9
0.1.9: Pulling from didil/hello-webhook
df20fa9351a1: Pull complete
c5ef763ec908: Pull complete
242c22b4c8b8: Pull complete
f201599daacc: Pull complete
Digest: sha256:8c6ef414e4df46f15b9b63f83bd24f7c6a2cc5b5b937c53f0811fd509969ad05
Status: Downloaded newer image for quay.io/didil/hello-webhook:0.1.9
quay.io/didil/hello-webhook:0.1.9
```

学习的环境

```
┌──[root@vms100.liruilongs.github.io]-[~]
└─$kubectl get node
NAME                          STATUS   ROLES           AGE    VERSION
vms100.liruilongs.github.io   Ready    control-plane   292d   v1.25.1
vms101.liruilongs.github.io   Ready    control-plane   292d   v1.25.1
vms102.liruilongs.github.io   Ready    control-plane   292d   v1.25.1
vms103.liruilongs.github.io   Ready    &lt;none&gt;          292d   v1.25.1
vms105.liruilongs.github.io   Ready    &lt;none&gt;          292d   v1.25.1
vms106.liruilongs.github.io   Ready    &lt;none&gt;          292d   v1.25.1
┌──[root@vms100.liruilongs.github.io]-[~]
└─$
```

看下作者的 yaml 文件，可以看到，当前作者使用 ku[stom](https://so.csdn.net/so/search?q=k8s&spm=1001.2101.3001.7020)ization 来管理 k8s yaml 资源文件, kustomize 是一个 生成 k8s 资源 yaml 文件的插件，支持 继承组合 等一些面向对象思维方面的 yaml 文件生成

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s]
└─$ls
csr  deployment  other
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s]
└─$tree .
.
├── csr
│   ├── csr-job.yaml
│   ├── csr-rolebinding.yaml
│   ├── csr-role.yaml
│   ├── csr-sa.yaml
│   └── kustomization.yaml
├── deployment
│   ├── deployment.yaml
│   └── kustomization.yaml
└── other
    ├── configmap.yaml
    ├── kustomization.yaml
    ├── service.yaml
    └── webhookconf.yaml

3 directories, 11 files
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s]
└─$



```

需要安装 kustomize ,嫌麻烦当然也可以直接看

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s]
└─$kustomize version
{Version:kustomize/v4.5.7 GitCommit:56d82a8378dfc8dc3b3b1085e5a6e67b82966bd7 BuildDate:2022-08-02T16:35:54Z GoOs:linux GoArch:amd64}
```

这里分别看下作者的三个目录里面放了什么

**deployment**** 目录**主要为注入逻辑的工作负载hello-webhook，这里需要注意的是挂载了一个证书文件，由于 Webhook 必须通过 HTTPS 提供，因此我们需要为服务器提供适当的自签名 CA 签名。证书的公用名 （CN） 必须与 Kubernetes API 服务器使用的服务器名称匹配。

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/deployment]
└─$ls
deployment.yaml  kustomization.yaml
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/deployment]
└─$kubectl kustomize ./
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: hello-webhook
  name: hello-webhook-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-webhook
  template:
    metadata:
      labels:
        app: hello-webhook
    spec:
      containers:
      - image: quay.io/didil/hello-webhook:0.1.9
        name: hello-webhook
        ports:
        - containerPort: 8000
        resources:
          limits:
            cpu: 500m
            memory: 128Mi
        volumeMounts:
        - mountPath: /tls
          name: hello-tls-secret
          readOnly: true
      volumes:
      - name: hello-tls-secret
        secret:
          secretName: hello-tls-secret
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/deployment]
└─$



```

下面为 webhook 的 核心逻辑,用于对 Pod 添加对应的 CM 卷 ,添加对应的操作记录，返回修改后的对象

```
func (app *App) HandleMutate(w http.ResponseWriter, r *http.Request) {
 admissionReview := &amp;admissionv1.AdmissionReview{}

 // read the AdmissionReview from the request json body
 err := readJSON(r, admissionReview)
 if err != nil {
  app.HandleError(w, r, err)
  return
 }

 // unmarshal the pod from the AdmissionRequest
 pod := &amp;corev1.Pod{}
 if err := json.Unmarshal(admissionReview.Request.Object.Raw, pod); err != nil {
  app.HandleError(w, r, fmt.Errorf(&quot;unmarshal to pod: %v&quot;, err))
  return
 }

 // add the volume to the pod
 pod.Spec.Volumes = append(pod.Spec.Volumes, corev1.Volume{
  Name: &quot;hello-volume&quot;,
  VolumeSource: corev1.VolumeSource{
   ConfigMap: &amp;corev1.ConfigMapVolumeSource{
    LocalObjectReference: corev1.LocalObjectReference{
     Name: &quot;hello-configmap&quot;,
    },
   },
  },
 })

 // add volume mount to all containers in the pod
 for i := 0; i &lt; len(pod.Spec.Containers); i++ {
  pod.Spec.Containers[i].VolumeMounts = append(pod.Spec.Containers[i].VolumeMounts, corev1.VolumeMount{
   Name:      &quot;hello-volume&quot;,
   MountPath: &quot;/etc/config&quot;,
  })
 }

 containersBytes, err := json.Marshal(&amp;pod.Spec.Containers)
 if err != nil {
  app.HandleError(w, r, fmt.Errorf(&quot;marshall containers: %v&quot;, err))
  return
 }

 volumesBytes, err := json.Marshal(&amp;pod.Spec.Volumes)
 if err != nil {
  app.HandleError(w, r, fmt.Errorf(&quot;marshall volumes: %v&quot;, err))
  return
 }

 // build json patch
 patch := []JSONPatchEntry{
  JSONPatchEntry{
   OP:    &quot;add&quot;,
   Path:  &quot;/metadata/labels/hello-added&quot;,
   Value: []byte(`&quot;OK&quot;`),
  },
  JSONPatchEntry{
   OP:    &quot;replace&quot;,
   Path:  &quot;/spec/containers&quot;,
   Value: containersBytes,
  },
  JSONPatchEntry{
   OP:    &quot;replace&quot;,
   Path:  &quot;/spec/volumes&quot;,
   Value: volumesBytes,
  },
 }

 patchBytes, err := json.Marshal(&amp;patch)
 if err != nil {
  app.HandleError(w, r, fmt.Errorf(&quot;marshall jsonpatch: %v&quot;, err))
  return
 }

 patchType := admissionv1.PatchTypeJSONPatch

 // build admission response
 admissionResponse := &amp;admissionv1.AdmissionResponse{
  UID:       admissionReview.Request.UID,
  Allowed:   true,
  Patch:     patchBytes,
  PatchType: &amp;patchType,
 }

 respAdmissionReview := &amp;admissionv1.AdmissionReview{
  TypeMeta: metav1.TypeMeta{
   Kind:       &quot;AdmissionReview&quot;,
   APIVersion: &quot;admission.k8s.io/v1&quot;,
  },
  Response: admissionResponse,
 }

 jsonOk(w, &amp;respAdmissionReview)
}



```

上述代码主要做了如下事情：

+   将来自 Http 请求中的 AdmissionReview json 输入反序列化。

+   读取 Pod 的 spec 信息。

+   将 hello-configmap 作为数据源，添加 hello-volume 卷到 Pod。

+   挂载卷至 Pod 容器中。

+   以 JSON PATCH 的形式记录变更信息，包括卷的变更，卷挂载信息的变更。顺道为容器添加一个“hello-added=true”的标签。

+   构建 json 格式的响应结果，结果中包含了这次请求中的被修改的部分。

**other**** 目****录主要放了 需要动态织入的**** CM ，以卷的方式使用，以及 MutatingWebhook**Configuration 准入控制器的定义，以及 webhook 的 SVC(SVC 名字太长，没办法生成证书，需要修改一下)

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/other]
└─$kubectl kustomize .
apiVersion: v1
data:
  hello.txt: &quot;\n /$$$$$$$$ /$$   /$$ /$$$$$$  /$$$$$$        /$$$$$$  /$$$$$$        /$$
    \  /$$  /$$$$$$   /$$$$$$ \n|__  $$__/| $$  | $$|_  $$_/ /$$__  $$      |_  $$_/
    /$$__  $$      | $$  /$$/ /$$__  $$ /$$__  $$\n   | $$   | $$  | $$  | $$  | $$
    \ \\__/        | $$  | $$  \\__/      | $$ /$$/ | $$  \\ $$| $$  \\__/\n   | $$
    \  | $$$$$$$$  | $$  |  $$$$$$         | $$  |  $$$$$$       | $$$$$/  |  $$$$$$/|
    \ $$$$$$ \n   | $$   | $$__  $$  | $$   \\____  $$        | $$   \\____  $$      |
    $$  $$   &gt;$$__  $$ \\____  $$\n   | $$   | $$  | $$  | $$   /$$  \\ $$        |
    $$   /$$  \\ $$      | $$\\  $$ | $$  \\ $$ /$$  \\ $$\n   | $$   | $$  | $$ /$$$$$$|
    \ $$$$$$/       /$$$$$$|  $$$$$$/      | $$ \\  $$|  $$$$$$/|  $$$$$$/\n   |__/
    \  |__/  |__/|______/ \\______/       |______/ \\______/       |__/  \\__/ \\______/
    \ \\______/ \n                                                                                                  \n
    \                                                                                                 \n
    \                                                                                                 \n&quot;
kind: ConfigMap
metadata:
  name: hello-configmap
---
apiVersion: v1
kind: Service
metadata:
  name: hello-webhook-service
spec:
  ports:
  - port: 443
    protocol: TCP
    targetPort: 8000
  selector:
    app: hello-webhook
  type: ClusterIP
---
apiVersion: admissionregistration.k8s.io/v1
kind: MutatingWebhookConfiguration
metadata:
  name: hello-webhook.leclouddev.com
webhooks:
- admissionReviewVersions:
  - v1
  - v1beta1
  clientConfig:
    service:
      name: hello-webhook-service
      namespace: default
      path: /mutate
  name: hello-webhook.leclouddev.com
  objectSelector:
    matchLabels:
      hello: &quot;true&quot;
  rules:
  - apiGroups:
    - &quot;&quot;
    apiVersions:
    - v1
    operations:
    - CREATE
    resources:
    - pods
    scope: Namespaced
  sideEffects: None
  timeoutSeconds: 10
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/other]
└─$



```

简单分析一下 MutatingWebhookConfiguration ,主要的配置 webhooks 配置

```
apiVersion: admissionregistration.k8s.io/v1
kind: MutatingWebhookConfiguration
metadata:
  name: hello-webhook.leclouddev.com
webhooks:
- admissionReviewVersions:
  - v1
  - v1beta1
  clientConfig: #指定了客户端配置，用于指定 Webhook 的服务信息。
    service:
      name: hello-webhook-service
      namespace: default
      path: /mutate
  name: hello-webhook.leclouddev.com #指定了 Webhook 的名称
  objectSelector: # 指定了对象选择器，用于选择要应用 Webhook 的对象。
    matchLabels:
      hello: &quot;true&quot;
  rules: #指定了 Webhook 的规则
  - apiGroups:
    - &quot;&quot;
    apiVersions:
    - v1
    operations:
    - CREATE
    resources:
    - pods
    scope: Namespaced
  sideEffects: None
  timeoutSeconds: 10 #指定了 Webhook 的超时时间，此处为 10 秒。



```

这里可以看到当前准入控制器 webhook 只处理 打了标签 hello=true 的 pod

**csr 目录为权限，生成证书的 Job，S**A 以及 通过 SA 添加对应的集群权限

生成的相关证书,用于 K8s Webhook 通信，相关项目地址：

[https://github.com/didil/k8s-](https://github.com/didil/k8s-webhook-cert-manager)webhook-cert-manager

当前作者的 csr-job.yaml 封装的 pod 比较旧，shell 脚本需要调整，所有我们不需要，直接本地生成 证书和对应的秘密

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$ls
csr-job.yaml  csr-rolebinding.yaml  csr-role.yaml  csr-sa.yaml  kustomization.yaml
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl kustomize  ./
apiVersion: v1
kind: ServiceAccount
metadata:
  name: webhook-cert-sa
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: webhook-cert-cluster-role
rules:
- apiGroups:
  - admissionregistration.k8s.io
  resources:
  - mutatingwebhookconfigurations
  verbs:
  - get
  - create
  - patch
- apiGroups:
  - certificates.k8s.io
  resources:
  - certificatesigningrequests
  verbs:
  - create
  - get
  - delete
- apiGroups:
  - certificates.k8s.io
  resources:
  - certificatesigningrequests/approval
  verbs:
  - update
- apiGroups:
  - &quot;&quot;
  resources:
  - secrets
  verbs:
  - create
  - get
  - patch
- apiGroups:
  - &quot;&quot;
  resources:
  - configmaps
  verbs:
  - get
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: webhook-cert-cluster-role-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: webhook-cert-cluster-role
subjects:
- kind: ServiceAccount
  name: webhook-cert-sa
  namespace: default
---
apiVersion: batch/v1
kind: Job
metadata:
  name: webhook-cert-setup
spec:
  backoffLimit: 3
  template:
    spec:
      containers:
      - args:
        - --service
        - hello-webhook-service
        - --webhook
        - hello-webhook.leclouddev.com
        - --secret
        - hello-tls-secret
        - --namespace
        - default
        command:
        - ./generate_certificate.sh
        image: quay.io/didil/k8s-webhook-cert-manager:0.13.19-1-a
        name: webhook-cert-setup
      restartPolicy: OnFailure
      serviceAccountName: webhook-cert-sa
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$



```

部署项目

在部署之前，我们需要改一下命名空间,创建一个新的命名空间， mutating-webhook

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl create  ns mutating-webhook
namespace/mutating-webhook created
```

然后修改一下每个目录的 kustomization.yaml 文件,类似下面这样，添加 namespace: mutating-webhook

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/other]
└─$cat kustomization.yaml
namespace: mutating-webhook
resources:
- configmap.yaml
- service.yaml
- webhookconf.yaml
```

然后还需要修改一下证书生成的的命名空间

```
./generate_certificate.sh  --service hello-webhook-service --webhook hello-webhook.leclouddev.com --secret hello-tls-secret --namespace mutating-webhook
```

之后就可以部署了

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl apply  -k ./
serviceaccount/webhook-cert-sa created
clusterrole.rbac.authorization.k8s.io/webhook-cert-cluster-role created
clusterrolebinding.rbac.authorization.k8s.io/webhook-cert-cluster-role-binding created
job.batch/webhook-cert-setup created
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl apply  -k ../other/
configmap/hello-configmap created
service/hello-webhook-service created
mutatingwebhookconfiguration.admissionregistration.k8s.io/hello-webhook.leclouddev.com created
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl apply  -k ../deployment/
deployment.apps/hello-webhook-deployment created
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl config  set-context --current --namespace mutating-webhook
Context &quot;kubernetes-admin@kubernetes&quot; modified.



```

webhook 一直没办法正常运行

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl get all
NAME                                            READY   STATUS              RESTARTS      AGE
pod/hello-webhook-deployment-7f599b95c4-pg9w5   0/1     ContainerCreating   0             55s
pod/webhook-cert-setup-v62rt                    0/1     CrashLoopBackOff    2 (15s ago)   79s

NAME                            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/hello-webhook-service   ClusterIP   10.103.24.30   &lt;none&gt;        443/TCP   59s

NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-webhook-deployment   0/1     1            0           55s

NAME                                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/hello-webhook-deployment-7f599b95c4   1         1         0       55s

NAME                           COMPLETIONS   DURATION   AGE
job.batch/webhook-cert-setup   0/1           79s        79s
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$



```

这里发现作者生成证书的 job 中脚本使用的 镜像中的 kubectl 版本太低了，没办法正常执行，所以我这么直接在的本地生成 证书，创建对应的 secret ,任然使用作者的 脚本 generate_certificate.sh

作者原来的 证书签名请求 对应的 API 资源对象使用的是 bata 版本，现在已经更新为正式版本，需要替换一下,其他部分逻辑也需要调整，下面为调整后的

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$cat generate_certificate.sh
#!/usr/bin/env sh

set -e

usage() {
  cat &lt;&lt;EOF
Generate certificate suitable for use with any Kubernetes Mutating Webhook.
This script uses k8s&#x27; CertificateSigningRequest API to a generate a
certificate signed by k8s CA suitable for use with any Kubernetes Mutating Webhook service pod.
This requires permissions to create and approve CSR. See
https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster for
detailed explantion and additional instructions.
The server key/cert k8s CA cert are stored in a k8s secret.
usage: ${0} [OPTIONS]
The following flags are required.
    --service          Service name of webhook.
    --webhook          Webhook config name.
    --namespace        Namespace where webhook service and secret reside.
    --secret           Secret name for CA certificate and server certificate/key pair.
The following flags are optional.
    --webhook-kind     Webhook kind, either MutatingWebhookConfiguration or
                       ValidatingWebhookConfiguration (defaults to MutatingWebhookConfiguration)
EOF
  exit 1
}

while [ $# -gt 0 ]; do
  case ${1} in
      --service)
          service=&quot;$2&quot;
          shift
          ;;
      --webhook)
          webhook=&quot;$2&quot;
          shift
          ;;
      --secret)
          secret=&quot;$2&quot;
          shift
          ;;
      --namespace)
          namespace=&quot;$2&quot;
          shift
          ;;
      --webhook-kind)
          kind=&quot;$2&quot;
          shift
          ;;
      *)
          usage
          ;;
  esac
  shift
done

[ -z &quot;${service}&quot; ] &amp;&amp; echo &quot;ERROR: --service flag is required&quot; &amp;&amp; exit 1
[ -z &quot;${webhook}&quot; ] &amp;&amp; echo &quot;ERROR: --webhook flag is required&quot; &amp;&amp; exit 1
[ -z &quot;${secret}&quot; ] &amp;&amp; echo &quot;ERROR: --secret flag is required&quot; &amp;&amp; exit 1
[ -z &quot;${namespace}&quot; ] &amp;&amp; echo &quot;ERROR: --namespace flag is required&quot; &amp;&amp; exit 1

fullServiceDomain=&quot;${service}.${namespace}.svc&quot;

# THE CN has a limit of 64 characters. We could remove the namespace and svc
# and rely on the Subject Alternative Name (SAN), but there is a bug in EKS
# that discards the SAN when signing the certificates.
#
# https://github.com/awslabs/amazon-eks-ami/issues/341
if [ ${#fullServiceDomain} -gt 64 ] ; then
  echo &quot;ERROR: common name exceeds the 64 character limit: ${fullServiceDomain}&quot;
  exit 1
fi

if [ ! -x &quot;$(command -v openssl)&quot; ]; then
  echo &quot;ERROR: openssl not found&quot;
  exit 1
fi

csrName=${service}.${namespace}
tmpdir=$(mktemp -d)
echo &quot;creating certs in tmpdir ${tmpdir} &quot;

cat &lt;&lt;EOF &gt;&gt; &quot;${tmpdir}/csr.conf&quot;
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name
[req_distinguished_name]
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names
[alt_names]
DNS.1 = ${service}
DNS.2 = ${service}.${namespace}
DNS.3 = ${fullServiceDomain}
DNS.4 = ${fullServiceDomain}.cluster.local
EOF
echo &quot;/CN=${fullServiceDomain}&quot;
openssl genrsa -out &quot;${tmpdir}/server-key.pem&quot; 2048
#openssl req -new -key &quot;${tmpdir}/server-key.pem&quot; -subj &quot;/CN=${fullServiceDomain}&quot; -out &quot;${tmpdir}/server.csr&quot; -config &quot;${tmpdir}/csr.conf&quot;
openssl req -new -key &quot;${tmpdir}/server-key.pem&quot; -subj &quot;/CN=system:node:${fullServiceDomain};/O=system:nodes&quot; -out &quot;${tmpdir}/server.csr&quot; -config &quot;${tmpdir}/csr.conf&quot;
set +e
# clean-up any previously created CSR for our service. Ignore errors if not present.
if kubectl delete csr &quot;${csrName}&quot;; then
    echo &quot;WARN: Previous CSR was found and removed.&quot;
fi
set -e

# create server cert/key CSR and send it to k8s api
cat &lt;&lt;EOF | kubectl create -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: ${csrName}
spec:
  #signerName: kubernetes.io/kube-apiserver-client
  signerName: kubernetes.io/kubelet-serving
  groups:
  - system:authenticated
  request: $(base64 &lt; &quot;${tmpdir}/server.csr&quot; | tr -d &#x27;\n&#x27;)
  usages:
  - server auth
  - digital signature
  - key encipherment
EOF

set +e
# verify CSR has been created
while true; do
  if kubectl get csr &quot;${csrName}&quot;; then
      echo &quot;CertificateSigningRequest create succsee&quot;
      break
  fi
done
set -e

# approve and fetch the signed certificate . !! not working with k8s 1.19.1, running the command separately outside of the container / node
set +e
while true; do
  if kubectl certificate approve &quot;${csrName}&quot;; then
     echo &quot;${csrName} certificate approve&quot;
     break
  fi
done

set -e

set +e
# verify certificate has been signed
i=1
while [ &quot;$i&quot; -ne 10 ]
do
  serverCert=$(kubectl get csr &quot;${csrName}&quot; -o jsonpath=&#x27;{.status.certificate}&#x27;)
  if [ &quot;${serverCert}&quot; != &#x27;&#x27; ]; then
      break
  fi
  sleep 5
  i=$((i + 1))
done

set -e
if [ &quot;${serverCert}&quot; = &#x27;&#x27; ]; then
  echo &quot;ERROR: After approving csr ${csrName}, the signed certificate did not appear on the resource. Giving up after 10 attempts.&quot; &gt;&amp;2
  exit 1
fi

echo &quot;${serverCert}&quot; | openssl base64 -d -A -out &quot;${tmpdir}/server-cert.pem&quot;

# create the secret with CA cert and server cert/key
kubectl create secret tls &quot;${secret}&quot; \
      --key=&quot;${tmpdir}/server-key.pem&quot; \
      --cert=&quot;${tmpdir}/server-cert.pem&quot; \
      --dry-run -o yaml |
  kubectl -n &quot;${namespace}&quot; apply -f -

#caBundle=$(base64 &lt; /run/secrets/kubernetes.io/serviceaccount/ca.crt  | tr -d &#x27;\n&#x27;)
caBundle=$(cat ${tmpdir}/server-cert.pem)
set +e
# Patch the webhook adding the caBundle. It uses an `add` operation to avoid errors in OpenShift because it doesn&#x27;t set
# a default value of empty string like Kubernetes. Instead, it doesn&#x27;t create the caBundle key.
# As the webhook is not created yet (the process should be done manually right after this job is created),
# the job will not end until the webhook is patched.
while true; do
  echo &quot;INFO: Trying to patch webhook adding the caBundle.&quot;
  if kubectl patch &quot;${kind:-mutatingwebhookconfiguration}&quot; &quot;${webhook}&quot; --type=&#x27;json&#x27; -p &quot;[{&#x27;op&#x27;: &#x27;add&#x27;, &#x27;path&#x27;: &#x27;/webhooks/0/clientConfig/caBundle&#x27;, &#x27;value&#x27;:&#x27;${serverCert}&#x27;}]&quot;; then
      break
  fi
  echo &quot;INFO: webhook not patched. Retrying in 5s...&quot;
  sleep 5
done
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$



```

生成证书，并且 创建 csr secret ，同时更新 mutatingwebhookconfiguration 的 caBundle 字段

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$./generate_certificate.sh  --service webhook-svc --webhook hello-webhook.leclouddev.com --secret hello-tls-secret --namespace mutating-webhook
creating certs in tmpdir /tmp/tmp.Di367dgyMz
/CN=webhook-svc.mutating-webhook.svc
Generating RSA private key, 2048 bit long modulus
........................................+++
.........+++
e is 65537 (0x10001)
Error from server (NotFound): certificatesigningrequests.certificates.k8s.io &quot;webhook-svc.mutating-webhook&quot; not found
certificatesigningrequest.certificates.k8s.io/webhook-svc.mutating-webhook created
NAME                           AGE   SIGNERNAME                      REQUESTOR          REQUESTEDDURATION   CONDITION
webhook-svc.mutating-webhook   0s    kubernetes.io/kubelet-serving   kubernetes-admin   &lt;none&gt;              Pending
CertificateSigningRequest create succsee
certificatesigningrequest.certificates.k8s.io/webhook-svc.mutating-webhook approved
webhook-svc.mutating-webhook certificate approve
W1115 17:25:40.403890   66211 helpers.go:663] --dry-run is deprecated and can be replaced with --dry-run=client.
secret/hello-tls-secret created
INFO: Trying to patch webhook adding the caBundle.
mutatingwebhookconfiguration.admissionregistration.k8s.io/hello-webhook.leclouddev.com patched



```

如果 命名空间或者 svc 名字太长的话，会报下面的错，需要调整短一点

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$./generate_certificate.sh  --service hello-webhook-service --webhook hello-webhook.leclouddev.com --secret hello-tls-secret --namespace k8s-hello-mutating-webhook
creating certs in tmpdir /tmp/tmp.GcNh5TwKXP
/CN=hello-webhook-service.k8s-hello-mutating-webhook.svc
Generating RSA private key, 2048 bit long modulus
....+++
...................................................+++
e is 65537 (0x10001)
problems making Certificate Request
140682165290896:error:0D07A097:asn1 encoding routines:ASN1_mbstring_ncopy:string too long:a_mbstr.c:158:maxsize=64
```

测试结果

创建指定标签的 Pod ,自动挂载 CM

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl run busybox-1 --image=busybox  --restart=Never -l=app=busybox,hello=true -- sleep 3600
pod/busybox-1 created
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl exec busybox-1 -it -- sh -c &quot;ls /etc/config/hello.txt&quot;
/etc/config/hello.txt
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl exec busybox-1 -it -- sh -c &quot;cat /etc/config/hello.txt&quot;

 /$$$$$$$$ /$$   /$$ /$$$$$$  /$$$$$$        /$$$$$$  /$$$$$$        /$$   /$$  /$$$$$$   /$$$$$$
|__  $$__/| $$  | $$|_  $$_/ /$$__  $$      |_  $$_/ /$$__  $$      | $$  /$$/ /$$__  $$ /$$__  $$
   | $$   | $$  | $$  | $$  | $$  \__/        | $$  | $$  \__/      | $$ /$$/ | $$  \ $$| $$  \__/
   | $$   | $$$$$$$$  | $$  |  $$$$$$         | $$  |  $$$$$$       | $$$$$/  |  $$$$$$/|  $$$$$$
   | $$   | $$__  $$  | $$   \____  $$        | $$   \____  $$      | $$  $$   &gt;$$__  $$ \____  $$
   | $$   | $$  | $$  | $$   /$$  \ $$        | $$   /$$  \ $$      | $$\  $$ | $$  \ $$ /$$  \ $$
   | $$   | $$  | $$ /$$$$$$|  $$$$$$/       /$$$$$$|  $$$$$$/      | $$ \  $$|  $$$$$$/|  $$$$$$/
   |__/   |__/  |__/|______/ \______/       |______/ \______/       |__/  \__/ \______/  \______/





```

没有标签的没有自动挂载

```
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl run busybox-2 --image=busybox --restart=Never -l=app=busybox -- sleep 3600
pod/busybox-2 created
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl get pod -l=app=busybox -L=hello-added
NAME        READY   STATUS    RESTARTS   AGE   HELLO-ADDED
busybox-1   1/1     Running   0          30m   OK
busybox-2   1/1     Running   0          28s
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$kubectl exec busybox-2 -it -- sh -c &quot;ls /etc/config/hello.txt&quot;
ls: /etc/config/hello.txt: No such file or directory
command terminated with exit code 1
┌──[root@vms100.liruilongs.github.io]-[~/ansible/k8s-hello-mutating-webhook/k8s-hello-mutating-webhook/k8s/csr]
└─$
```

###   博文部分内容参考

© 文中涉及参考链接内容版权归原作者所有，如有侵权请告知，这是一个开源项目，如果你认可它，不要吝啬星星哦 😃

---

[ht](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#service-reference)tps://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/#service-reference

[https://kubernetes.io/zh-cn/docs/reference/kubern](https://kubernetes.io/zh-cn/docs/reference/kubernetes-api/extend-resources/mutating-webhook-configuration-v1/)etes-api/extend-resources/mutating-webhook-configuration-v1/

[h](https://cloudnative.to/blog/mutating-admission-webhook/)ttps://cloudnative.to/blog/mutating-admission-webhook/

[https://github.com/didil/k8s-he](https://github.com/didil/k8s-hello-mutating-webhook)llo-mutating-webhook

[https://didil.medium.com/building-a-kubernetes](https://didil.medium.com/building-a-kubernetes-mutating-admission-webhook-7e48729523ed)-mutating-admission-webhook-7e48729523ed

---

© 2018-2023 liruilonger@gmail.com , All rights reserved. 保持署名-非商用-相同方式共享(CC BY-NC-SA 4.0)