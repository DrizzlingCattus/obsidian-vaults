# JSON column

ref: [](https://dev.mysql.com/doc/refman/8.0/en/json.html)[https://dev.mysql.com/doc/refman/8.0/en/json.html](https://dev.mysql.com/doc/refman/8.0/en/json.html)

mysql은 json type field를 지원함.

json type field는 다음과 같은 이점이 있음.

-   automatic validation of json documents
-   optimized storage format. 내부적으로 quick read access가 가능한 형태로 format을 변경해서 저장해둠.

mysql 8.0에서는 normalize, merging, autowrapping을 지원

normalize는 중복되는 키를 제거하는 작업. 마지막에 삽입된 키가 최종 출력 값임. ('last duplicate key win!')

단, mysql 8.0.3 이전 버전은 전략이 다름. 'first duplicate key win' 전략임...

그 외에도 whitespace를 제거 해주는 등 가독성을 위한 몇 가지 작업을 해줌.

To make lookups more efficient, MySQL also sorts the keys of a JSON object. _You should be aware that the result of this ordering is subject to change and not guaranteed to be consistent across releases_.

[](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-patch)[https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-patch](https://dev.mysql.com/doc/refman/8.0/en/json-modification-functions.html#function_json-merge-patch)