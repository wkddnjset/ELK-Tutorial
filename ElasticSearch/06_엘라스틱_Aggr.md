# Elastic Search Aggregation 기능

Aggregation 종류는 다음과 같습니다.
- Metrics Aggregation
- Bucket Aggregation
- Pipeline Aggregation
- Matrix Aggregation
> 자세한 내용은 [여기](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)를 참고하세요.

## Metric Aggregation

```bash
root@eafa6d164a7e:/# curl -H "Content-Type:application/json" -XPOST http://localhost:9200/_bulk?pretty --data-binary @BigData/ch03/simple_basketball.json
```
`simple_basketball.json` 파일에 있는 농구 경기에 대한 데이터를 입력 합니다.

```bash
root@eafa6d164a7e:/# vi BigData/ch03/avg_points_aggs.json
```
농구 경기에 평균 점수를 구하는 Aggregation 입니다.
> vi/vim은 terminal에서 파일을 수정하거나 확인할 때 사용합니다.

```json
{
        "size" : 0,
        "aggs" : {
                "avg_score" : {
                        "avg" : {
                                "field" : "points"
                        }
                }
        }
}
```
- avg [평균]
- min [최소]
- max [최대]
- sum [총합]
- stats [전체]

```bash
root@eafa6d164a7e:/# curl -H "Content-Type:application/json" -XPOST http://localhost:9200/_search?pretty --data-binary @BigData/ch03/avg_points_aggs.json
```

## Bucket Aggregation (Group by)

```bash
root@eafa6d164a7e:/# curl -XDELETE http://localhost:9200/basketball?pretty
```
기존에 테스트를 위해 넣었던 데이터를 삭제합니다.

```bash
root@eafa6d164a7e:/# curl -XPUT http://localhost:9200/basketball?pretty
```
`basketball`이라는 인덱스(테이블)을 만들어줍니다.

```bash
root@eafa6d164a7e:/# curl -H "Content-Type:application/json" -XPUT http://localhost:9200/basketball/record/_mapping?pretty -d @BigData/ch04/basketball_mapping.json
```
해당 인덱스에 `mapping`을 합니다.

```bash
root@eafa6d164a7e:/# curl -H "Content-Type:application/json" -XPOST http://localhost:9200/_bulk?pretty --data-binary @BigData/ch04/twoteam_basketball.json
```
`twoteam_basketball.json` 파일에 있는 농구 경기에 대한 데이터를 입력 합니다.

### terms_aggs.json
```json
{
        "size" : 0,
        "aggs" : {
                "players" : {
                        "terms" : {
                                "field" : "team"
                        }
                }
        }
}
```
- `players` : Aggregation 이름
- `terms` : `GROUP BY`와 동일한 명령어
- `field : team` : `TEAM`을 기준으로 GROUP BY

### stats_by_team.json
```json
{
        "size" : 0,
        "aggs" : {
                "team_stats" : {
                        "terms" : {
                                "field" : "team"
                        },
                        "aggs" : {
                                "stats_score" : {
                                        "stats" : {
                                                "field" : "points"
                                        }
                                }
                        }
                }
        }
}
```
`TEAM`을 기준으로 `GROUP BY`를 하고 `TEAM`별 `POINTS`에 대한 통계를 보여주는 Aggregation 입니다.
