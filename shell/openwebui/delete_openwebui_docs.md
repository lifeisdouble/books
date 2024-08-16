```shell
#!/bin/bash

site_url="http://xxx:8080"
site_docs_url="http://xxx:8080/api/v1/documents/"
site_delete_doc_url="xxx:8080/api/v1/documents/doc/delete?name="
token="eyJ_xxx_2bsRU62R9kDw"

# Fetch the JSON data using curl
resp=$(curl $site_docs_url \
              -H 'Accept: application/json' \
              -H 'Accept-Language: zh-CN,zh;q=0.9' \
              -H 'Connection: keep-alive' \
              -H 'Content-Type: application/json' \
              -H 'Cookie: token='$token \
              -H 'Referer: '$site_docs_url \
              -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36' \
              -H 'authorization: Bearer '$token \
              --insecure)

NAMES=$(jq -r '.[] | .name' <<< "$resp")

# 打印提取的name字段
echo "Extracted names:"
for name in $NAMES; do

#    echo "$site_delete_doc_url$name"
    resp=$(curl $site_delete_doc_url$name \
      -X 'DELETE' \
      -H 'Accept: application/json' \
      -H 'Accept-Language: zh-CN,zh;q=0.9' \
      -H 'Connection: keep-alive' \
      -H 'Content-Type: application/json' \
      -H 'Cookie: token='$token \
      -H 'Origin: '$site_url \
      -H 'Referer: $site_url/workspace/documents' \
      -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36' \
      -H 'authorization: Bearer '$token \
      --insecure)
    echo "delete doc : " $name " result is " $resp

done
```
