# ElasticSearch RestAPI

`Talend API Tester` 등을 이용하여 REST API 테스트를 할 수 있다.

엘라스틱서치 검색은 전부 `REST API` 를 통해서 이루어 진다.

## 클러스터에 존재하는 모든 인덱스 조회

GET : `http://localhost:9200/_cat/indices?v`

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
