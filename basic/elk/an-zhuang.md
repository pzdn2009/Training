# 安裝

## 安裝

## 1. 源碼安裝方式

### 1.1 Logstash

* 下載Logstash并解壓

  ```text
  wget https://artifacts.elastic.co/downloads/logstash/logstash-5.1.1.tar.gz
  tar vxf logstash-5.1.1
  cd logstash-5.1.1
  ```

* 準備一個logstash使用的配置文件

  ```text
  input { stdin { } }
  output {
  elasticsearch { hosts => ["localhost:9200"] }
  stdout { codec => rubydebug }
  }
  ```

* 運行

  ```text
  bin/logstash -f logstash.conf
  ```

  **1.2 Elasticsearch**

* 下載并解壓Elasticsearch

  ```text
  wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.1.1.tar.gz
  tar vxf elasticsearch-5.1.1
  cd elasticsearch-5.1.1
  ```

* 運行

  ```text
  bin/elasticsearch
  curl http://localhost:9200/
  ```

### 1.3 Kibana

* 下載Kibana壓縮包并解壓

  ```text
  wget https://artifacts.elastic.co/downloads/kibana/kibana-5.1.1-linux-x86_64.tar.gz
  tar vxf kibana-5.1.1-linux-x86_64.tar.gz
  cd kibana-5.1.1-linux-x86_64
  ```

* 在編輯器中打卡config/kibana.yml

  ```text
  vim config/kibana.yml
  ```

* 設置elasticsearch.url 指向Elasticsearch實例

  ```text
  elasticsearch.url: "http://localhost:9200"
  ```

* 運行bin/kibana \(or bin\kibana.bat on Windows\)

  ```text
  /bin/kibana
  ```

* 瀏覽器中打開[http://localhost:5601](http://localhost:5601)

