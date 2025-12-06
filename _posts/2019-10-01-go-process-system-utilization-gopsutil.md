---
layout: post
title: Golang psutil 포팅된 gopsutil 라이브러리 알아보기
---

오늘은 Golang으로 프로세스와 시스템에 대한 정보를 보여주는 psutil을 포팅한 gopsutil 라이브러리를 알아보려 합니다.

## gopsutil 설치

우선 Golang의 환경을 구성하기 위해 https://golang.org/dl/ 에서 윈도우, 리눅스, 맥에서 설치 프로그램을 내려받을 수 있습니다.

맥에서 brew로 쉽게 설치할 수 있습니다.

```
brew install go
```

우분투에서도 apt로 쉽게 설치할 수 있습니다.

```
sudo apt-get install golang-go
```

맥에서 Golang의 버전을 올리려면 brew를 이용합니다.

```
brew upgrade go
```

우분투에서도 Golang의 버전을 올리려면 backports 저장소를 등록하고 apt를 이용합니다.

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt-get update
sudo apt-get install golang-go
```

go get으로 gopsutil 패키지를 설치합니다.

```
go get github.com/shirou/gopsutil 
```

홈의 go 폴더에 gopsutil 소스코드와 패키지 파일이 생성됩니다.

## 예제

```go
package main
```

해당 소스코드를 실행 파일로 인식하게 해주도록 main이라고 선언합니다.

```go
import (
	"fmt"

	"github.com/shirou/gopsutil/cpu"
	"github.com/shirou/gopsutil/disk"
	"github.com/shirou/gopsutil/docker"
	"github.com/shirou/gopsutil/host"
	"github.com/shirou/gopsutil/load"
	"github.com/shirou/gopsutil/mem"
	"github.com/shirou/gopsutil/net"
	"github.com/shirou/gopsutil/process"
)
```

fmt와 gopsutil에서 cpu, disk, docker, host, load, mem, net, process를 가져옵니다.

```go
func main() {
	virtualMemory, err := mem.VirtualMemory()
	if err != nil {
		panic(err)
	}
	fmt.Println(virtualMemory)
```

gopsutil의 mem에서 가상 메모리에 대한 정보를 반환하여 출력합니다.

```go
	cpuInfo, err := cpu.Info()
	if err != nil {
		panic(err)
	}
	fmt.Println(cpuInfo, cpuCounts, cpuTimes)
```

gopsutil의 cpu에서 cpu에 대한 정보를 반환하여 출력합니다.

```go
	loadAvg, err := load.Avg()
	if err != nil {
		panic(err)
	}
	fmt.Println(loadAvg)
```

gopsutil의 load에서 커널의 load 값에 대한 평균 정보를 반환하여 출력합니다.

```go
	diskPartitions, err := disk.Partitions(true)
	if err != nil {
		panic(err)
	}
	fmt.Println(diskPartitions)
```

gopsutil의 disk에서 파티션에 대한 정보를 반환하여 출력합니다.

```go
	processes, err := process.Processes()
	if err != nil {
		panic(err)
	}
	processPid, err := process.Pids()
	if err != nil {
		panic(err)
	}
	fmt.Println(processes, processPid)
```

gopsutil의 process에서 프로세스 목록을 반환하여 출력합니다.

```go
	hostInfo, err := host.Info()
	if err != nil {
		panic(err)
	}
	fmt.Println(hostInfo)
```

gopsutil의 host에서 호스트에 대한 정보를 반환하여 출력합니다.

```go
	dockerIDList, err := docker.GetDockerIDList()
	if err != nil {
		panic(err)
	}
	dockerStat, err := docker.GetDockerStat()
	if err != nil {
		panic(err)
	}
	fmt.Println(dockerIDList, dockerStat)
```

gopsutil의 docker에서 도커에 대한 상태 정보를 반환하여 출력합니다.

```go
	netInterfaces, err := net.Interfaces()
	if err != nil {
		panic(err)
	}
	fmt.Println(netInterfaces)
}
```

gopsutil의 net에서 네트워크 인터페이스 정보를 반환하여 출력합니다.
