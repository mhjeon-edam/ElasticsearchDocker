## Elastic Kibana 사용법 ## (REST API 사용)

1. localhost:5601 접속
2. elastic 왼쪽 로고 아래 목록 버튼 -> Management -> Dev Tools 선택
3. Console 창에 쿼리 작성 -> Ctrl+Enter / 시작버튼 클릭 -> 오른쪽 결과창 결과확인

## 예제 ##

## 1. index 생성
PUT /products

## 2. mapping
PUT /products/_mappings
{
  "properties": {
    "name": {
      "type":"text"
    }
  }
}

## 3. document insert
POST /products/_doc
{
  "name" : "Apple 2026 맥북 에어 13 M4 10코어"
}

## 3-1. document ID 지정
POST /products/_create/1
{
  "name": "Apple 2024 맥북"
}

## 4. search
GET /products/_search
{
  "query": {
    "match": {
      "name": "맥북 13 에어 M4"
    }
  }
}

## 5. Modify
PUT /products/_doc/1
{
  "name": "apple 2024 맥북2"
}

POST /products/_update/1
{
  "doc": {
    "name" : "Apple 2026 맥북3"
  }
}

## 6. Delete
DELETE /products/_doc/1

## 7. _analyze
## char_filter (html_strip / mapping / pattern_replace)
## tokenizer (standard / keyword / simple / whitespace / uax_url_email / ngram / edge_ngram / letter / nori_tokenizser)
## filter (lowercase / uppercase / stop / stemmer / synonym / ngram / length / word_delimiter/ nori_part_of_speech / nori_reading_form )
GET /_analyze
{
  "text": "Apple 2026 맥북 에어 13 M4 10코어",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["lowercase"]
}

## 7-1 custom analyze 
PUT /products2
{
  "settings": {
    "analysis": {
      "analyzer": {
        "products_name_analyzer": {
          "char_filter": [],
          "tokenizer": "standard",
          "filter": ["lowercase"]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "name": {
        "type": "text",
        "analyzer": "products_name_analyzer"
      }
    }
  }
}

POST /products2/_create/1
{
  "name" : "Apple 2026 맥북 에어 13 M4 10코어"
}

GET /products2/_search
{
  "query": {
    "match": {
      "name": "apple"
    }
  }
}

GET /products2/_analyze
{
  "field" : "name",
  "text" : "Apple 2026 맥북 에어 13 M4 10코어"
}
