---
layout: post
title: 파이썬 aiobotocore 패키지 간단히 알아보기
---

오늘은 파이썬으로 비동기를 구현할 때 사용하는 asyncio 패키지와 boto에 비동기 지원하는 aiobotocore 패키지에 대하여 실습해보려 합니다.

우선 asyncio에 대하여 간단히 알아보자면, async await 로 동시성 코드를 작성할 수 있는 라이브러리입니다.

Python 3.6 이상 버전이면 기본적으로 존재합니다.

더 많은 정보는 아래 문서를 참조할 수 있으며, 간단한 예제로 보겠습니다.

https://docs.python.org/ko/3/library/asyncio.html

```python
import asyncio

async def main():
    wait asyncio.sleep(1)

asyncio.run(main())
```

asyncio 패키지를 import하고 async 문법으로 코루틴을 선언합니다.

asyncio.run 으로 asyncio 이벤트 루프를 관리합니다.

```python
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
loop.close()
```

만약 3.6 버전이라면, main 코루틴이 끝나기 전에 잠시 기다릴 수 있도록 로우레벨 API로 이벤트 루프를 사용해야 합니다. 

## aiobotocore 개요

aiobotocore는 aiohttp와 asyncio를 사용하는 Amazon Boto 비동기 지원 패키지입니다.

현재는 S3를 공식적으로 지원한다고 하므로, 예제도 aws S3 위주로 보려고 합니다.

## aiobotocore 설치

우선 virtualenv로 파이썬 환경을 분리해줍니다.

```
pip3 install virtualenv
```

```
virtualenv -mvenv env
```

env라는 이름의 가상 환경을 생성합니다.

```
source env/bin/activate
```

가상환경을 폴더에서 활성화합니다.

```
pip3 install --upgrade pip
```

pip의 업그레이드가 존재하는지 확인하고 진행합니다.

```
pip install aiobotocore
```

pip로 aiobotocore 설치합니다.

https://pypi.org/project/aiobotocore/


## 예제

우선 get_session으로 세션을 만들고, 만들어진 세션으로 s3 클라이언트를 준비합니다.

로컬 aws 디렉토리에서 aws credentials 파일을 찾거나, aws_secret_access_key와 aws_access_key_id 속성으로 입력할 수 있습니다.

```python
session = aiobotocore.get_session()
async with session.create_client('s3',
                                aws_secret_access_key=AWS_KEY,aws_access_key_id=AWS_KEY_ID) as client:
```

특정 버킷의 파일 목록을 가져옵니다.

boto에서 모든 파일 목록을 가져올 때 list_objects를 사용하게 된다면, 파일이 많은 경우에는 일부 잘려서 가져오게 되므로 paginator를 사용합니다.

버킷명과 접두사를 지정하여 가져오고 싶은 경로의 파일 목록을 가져옵니다.

async for로 비동기 함수를 순차적으로 실행하도록 합니다. 이렇게 S3 버킷의 파일 목록은 순서대로 출력할 수 있게 됩니다.

```python
paginator = client.get_paginator('list_objects')
async for result in paginator.paginate(Bucket=bucket, Prefix=prefix):
    for content in result.get('Contents', []):
        print(content)
```

데이터를 특정 버킷에서 가져옵니다.

비동기 Context Manager로 데이터를 받습니다.

```python
response = await client.get_object(Bucket=bucket, Key=key)
async with response["Body"] as stream:
    data = await stream.read()
```

데이터를 특정 버킷에 키 경로 지정하여 저장합니다.

await 문법으로 put_object를 수행합니다.

```python
await client.put_object(Bucket=bucket,
                        Key=key,
                        Body=data)
```

특정 버킷의 특정 키 경로에 있는 파일 오브젝트를 삭제합니다.

await 문법으로 delete_object를 수행합니다.

```python
await client.delete_object(Bucket=bucket, Key=key)
```

