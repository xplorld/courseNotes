# containers
- container: Docker
- registry<Docker>: where you pull docker, gcr/azure/...
- orchestration: kubernetes
- k8s template engine: helm, ksonnet
- k8s custom controller: run on a pod, do something when some event happens to my CRD

# tf
- model server: web server, called by rpc or http, do inference
- distributed tf: ps and worker

# kubeflow
- jyupter hub: a pod of a notebook server, each notebook can run on gpu pods
- tfjob controller: k8s custom controller to CRUD tfjobs
    - a tfjob = a docker of my tf python script + config of gpu/cpu usage + more configs
    - add a tfjob, kubeflow ctrler will create (gpu) pods for me to train
    - can monitor progress through `kubectl` or `ks`
- tf serving: a tf model server
    - difference from official tf server: model is loaded from online storage
    - deployed to k8s

# workflow

所以日常就是面对一个已经 deploy 好的 kubeflow,
1. 在 notebook 上灵魂写代码
2. 跑了一下差不多，用 kubeflow 开个 tfjob
3. 没事用 kubectl 看看进度
4. 训练完了还行，上传 model 到奇怪的 cloud storage, 通知一下 tf serving
5. loop
