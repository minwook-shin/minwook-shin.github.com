---
layout: post
title: 맥에 로컬로 minikube 설치하고 node 환경 실습해보기
---

오늘은 맥에서 로컬로 minikube를 설치하고 node 환경을 구동해보려 합니다.

## 개요

쿠버네티스는 여러 가상 컨테이너를 관리해주며, 이를 로컬 환경에서 구동하기 위해 minikube가 필요합니다.

## 가상화 확인

해당 컴퓨터의 cpu가 가상화 기술을 지원해야 합니다.

## kubernetes-cli 설치

kubernetes-cli로 디폴로이먼트를 생성할 수 있습니다.

```
brew install kubernetes-cli
```

맥에서는 kubernetes-cli을 brew로 설치할 수 있습니다.

```
kubectl version
```

kubernetes-cli의 버전을 확인합니다.

## 가상 머신 설치

virtual box 혹은 [HyperKit](https://github.com/moby/hyperkit)을 설치해서 가상 머신을 준비합니다.

## minikube 설치

```
brew cask install minikube
```

minikube를 brew로 설치합니다.

```
minikube start
```

minikube를 시작합니다.

```
➜  ~ minikube start
😄  minikube v1.2.0 on darwin (amd64)
🔥  Creating virtualbox VM (CPUs=2, Memory=2048MB, Disk=20000MB) ...
🐳  Configuring environment for Kubernetes v1.15.0 on Docker 18.09.6
💾  Downloading kubeadm v1.15.0
💾  Downloading kubelet v1.15.0
🚜  Pulling images ...
🚀  Launching Kubernetes ... 
⌛  Verifying: apiserver proxy etcd scheduler controller dns
🏄  Done! kubectl is now configured to use "minikube"
➜  ~ 
```

minikube를 시작하면 미리 설치한 virtualbox의 가상머신이 구동되면서 Kubernetes가 설정됩니다.

이 이후에 minikube 명령어를 쓸 수 있습니다.

## ssh

```
➜  ~ minikube ssh
                         _             _            
            _         _ ( )           ( )           
  ___ ___  (_)  ___  (_)| |/')  _   _ | |_      __  
/' _ ` _ `\| |/' _ `\| || , <  ( ) ( )| '_`\  /'__`\
| ( ) ( ) || || ( ) || || |\`\ | (_) || |_) )(  ___/
(_) (_) (_)(_)(_) (_)(_)(_) (_)`\___/'(_,__/'`\____)

$ uname -a
Linux minikube 4.15.0 #1 SMP Sun Jun 23 23:02:01 PDT 2019 x86_64 GNU/Linux
```

minikube 가상 머신에 ssh로 접근할 수도 있습니다.


## 디폴로이먼트

```
➜  ~ kubectl create deployment node-test --image=gcr.io/hello-minikube-zero-install/hello-node
```

kubernetes-cli로 node-test라는 디폴로이먼트를 생성할 수 있습니다.

해당 실습에서는 gcr.io/hello-minikube-zero-install/hello-node 이미지를 사용합니다.

```
➜  ~ kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
node-test    0/1     1            0           6s
```

디폴로이먼트를 확인할 수 있습니다.

```
➜  ~ kubectl get pods
NAME                          READY   STATUS              RESTARTS   AGE
node-test-55b49fb9f8-5b6jj    0/1     ContainerCreating   0          17s
```

배포할 수 있는 객체인 pod을 확인할 수 있습니다.

## 서비스

```
kubectl expose deployment node-test --type=LoadBalancer --port=8080
```

디플로이먼트를 외부로 노출시키는 서비스를 생성할 수 있습니다.

```
minikube service node-test --url
```

서비스에 접근할 수 있는 주소가 출력됩니다.

## addon

대시보드나 리소스 모니터링을 할 수 있는 애드온을 관리하려 합니다.

```
minikube addons list
```

해당 minikube에서 실행되는 애드온의 리스트를 출력합니다.

```
- addon-manager: enabled
- dashboard: disabled
- default-storageclass: enabled
- efk: disabled
- freshpod: disabled
- gvisor: disabled
- heapster: disabled
- ingress: disabled
- logviewer: disabled
- metrics-server: disabled
- nvidia-driver-installer: disabled
- nvidia-gpu-device-plugin: disabled
- registry: disabled
- registry-creds: disabled
- storage-provisioner: enabled
- storage-provisioner-gluster: disabled
```

## heapster

```
minikube addons enable heapster
```

모니터링을 하기 위해 heapster라는 애드온 활성화합니다.

```
minikube addons open heapster
```

heapster를 브라우저에서 실행할 수 있습니다.

## dashboard

웹 UI에서 쿠버네티스를 관리할 수 있게 대시보드를 사용할 수 있습니다.

```
minikube dashboard
```

대시보드가 활성화되어 있지 않아도 위 명령어로 한번에 구동할 수 있습니다.

대시보드에 접속하면 디폴로이먼트를 생성하거나, pod의 콘솔로 들어갈 수 있습니다.

```js
root@node-test-55b49fb9f8-5b6jj:/# cat server.js 
var http = require('http');

var handleRequest = function(request, response) {
  console.log('Received request for URL: ' + request.url);
  response.writeHead(200);
  response.end('Hello World!');
};
var www = http.createServer(handleRequest);
www.listen(8080);
```

node-test라는 pod의 콘솔에서 ls 명령어로 server.js 파일이 위와 같음을 확인할 수 있습니다.

## 중지

```
➜  ~ minikube stop
✋  Stopping "minikube" in virtualbox ...
🛑  "minikube" stopped.
```

minikube로 stop하면 virtualbox 가상머신이 중지됩니다.

## 정리

서비스를 삭제하려면 아래와 같이 서비스를 지정합니다.

```
kubectl delete services node-test
```

실행 중인 디플로이먼트와 팟을 삭제하려면 아래와 같이 지정합니다.
    
```
kubectl delete deployment node-test
```