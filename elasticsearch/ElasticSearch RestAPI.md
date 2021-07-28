# ElasticSearch RestAPI

`Talend API Tester` 등을 이용하여 REST API 테스트를 할 수 있다.

엘라스틱서치 검색은 전부 `REST API` 를 통해서 이루어 진다.

## 클러스터에 존재하는 모든 인덱스 조회

GET : `http://localhost:9200/_cat/indices?v`

- RESULT

결과는 아래와 같은 형태로 나온다.

```
health status index   uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   menu   Mq0MIU6UT76mYfOAvhH_pQ   1   0       1132            0    283.5kb        283.5kb
green  open   contents   ND-u0s0LSpqzl02UWJlpJw   1   0        878            0    177.3kb        177.3kb
green  open   media   S64FmICRQQ-SnpP8auHL9g   1   0       8695            0     23.1mb         23.1mb
green  open   keyword Ohpb58r7RH-SKJ-nHcmILA   1   0        302            0     26.3kb         26.3kb
green  open   file   Y9T6C2yERQWJ2F_q8-KvcA   1   0      12234            0      3.5mb          3.5mb
green  open   article   jQ0a2JSqTyyjKk7a4AD0Zg   1   0      21846            0     32.1mb         32.1mb
```

## 검색 API 테스트

POST : `http://localhost:9200/bdata/_search`
BODY : `SearchSourceBuilder` 로 생성한 쿼리(JSON)

- body example

```json
{
	"from": 0,
	"size": 3,
	"query": {
		"bool": {
			"must": [{
				"multi_match": {
					"query": "REST API 테스트",
					"fields": ["content^2.0", "title^1.0"],
					"type": "best_fields",
					"operator": "OR",
					"slop": 0,
					"prefix_length": 0,
					"max_expansions": 50,
					"zero_terms_query": "NONE",
					"auto_generate_synonyms_phrase_query": true,
					"fuzzy_transpositions": true,
					"boost": 1.0
				}
			}],
			"adjust_pure_negative": true,
			"boost": 1.0
		}
	},
	"_source": {
		"includes": ["url", "title", "content", "regdate", "menuname"],
		"excludes": []
	},
	"sort": [{
		"_score": {
			"order": "desc"
		}
	}, {
		"regdate": {
			"order": "desc"
		}
	}],
	"highlight": {
		"pre_tags": ["<font color='#db7c00'>"],
		"post_tags": ["</font>"],
		"fragment_size": 150,
		"fields": {
			"content": {}
		}
	}
}
```

- "fields": `["content^2.0", "title^1.0"]` 여기서 숫자는 가중치를 의미한다. 정확도(score) 산출할 때 영향을 미친다.
