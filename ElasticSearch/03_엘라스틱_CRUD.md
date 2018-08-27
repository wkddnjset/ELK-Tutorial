# ElasticSearch CRUD


```bash
$ docker exec -it 5029351fa373 /bin/bash
```
도커를 사용해 우분투를 실행합니다.

## Index 조회 [GET]

```bash
root@5029351fa373:/# cd ElasticSearch
root@5029351fa373:/ElasticSearch# curl -XGET http://localhost:9200/classes
```
Elastic Search가 설치된 폴더로 이동하여 `classes`란 인덱스를 호출합니다.
> 아직 어떤 데이터도 입력되지 않은 상태여서 `404 에러`가 뜰겁니다.

* `curl -XGET http://localhost:9200/classes?pretty` 이렇게 뒤에 `?prettt`를 추가하면 `json`형식으로 보여집니다.

## Index 생성 [PUT]

```bash
root@5029351fa373:/ElasticSearch# curl -XPUT http://localhost:9200/classes
{"acknowledged":true,"shards_acknowledged":true,"index":"classes"}
```
`index`명이 classes라는 데이터가 생성되었습니다.

```bash
root@5029351fa373:/ElasticSearch# curl -XGET http://localhost:9200/classes
```
다시 `classes`룰 GET 해보면 `404 에러`가 뜨지 않는걸 확인할 수 있습니다.

## Index 삭제 [DELETE]

```bash
root@5029351fa373:/ElasticSearch# curl -XDELETE http://localhost:9200/classes
{"acknowledged":true}
```
정상적으로 삭제가 되었습니다.

```bash
root@5029351fa373:/ElasticSearch# curl -XGET http://localhost:9200/classes
```
다시 `classes`룰 GET 해보면 `404 에러`가 뜨게 됩니다.

## Document 생성 [POST]

DB에 필드에 해당하는 Document를 생성하는 명령어 입니다.

```bash
root@5029351fa373:/ElasticSearch# curl -XPOST http://localhost:9200/classes/class/1 -d '{"title":"Algorithm", "professor":"John"}'
```
Document는 `json` 형태로 입력해야 합니다.

```bash
root@5029351fa373:/ElasticSearch# curl -XPOST http://localhost:9200/classes/class/1 -d @oneclass.json
```
다음과 같이 `json` 파일형태로 Document를 생성할 수 있습니다.
> 파일로 입력을 하기 위해 github을 사용합니다. git 명령어를 사용하기 위해서는 `sudo apt-get install git`으로 패키지 설치를 해주셔야합니다.

```bash
root@5029351fa373:/ElasticSearch# git clone https://github.com/minsuk-heo/BigData
root@5029351fa373:/ElasticSearch# ls
bigData elasticsearch-6.4.0.deb  elasticsearch-6.4.0.deb.sha512
```
`git clone`을 하면 `bigData`폴더가 설치됩니다. 

```bash
root@5029351fa373:/ElasticSearch# curl -H "Content-Type:application/json" -XPOST http://localhost:9200/classes/class/1/?pret
ty -d @BigData/ch01/oneclass.json
```
입력할 파일 포멧이 `json`이기 때문에 헤더 부분에 `Content-Type:application/json`을 넣어서 POST를 하면 데이터가 입력됩니다.

```bash
root@5029351fa373:/ElasticSearch# curl -XGET http://localhost:9200/classes/class/1/?pretty
```
`GET`으로 데이터가 잘 들어갔는지 확인합니다.

## Document 업데이트 [POST]

```bash
root@5029351fa373:/ElasticSearch# curl -H "Content-Type:application/json" -XPOST http://localhost:9200/classes/class/1 -d '{"title":"Algorithm", "professor":"John"}'
```
우선 Documnet를 생성합니다.

```bash
root@5029351fa373:/ElasticSearch# curl -H "Content-Type:application/json" -XPOST http://localhost:9200/classes/class/1/_update -d '{"doc":{"unit":1}}'
```
기존 생성된 Documnet에 `unit`이라는 필드를 추가합니다..
