# usage
- 간단하게 class 속성처럼 dot access를 사용하고 싶을 때 사용.

# how-to
## namedtuple key 값 얻기 #anki
```python
from collections import namedtuple

DeliveryInfo = namedtuple(
    "DeliveryInfo",
    [
        "uuid",
        "accepted_dtm",
    ],
)

# (uuid, accepted_dtm)
print(DeliveryInfo._fields)

```
ref: https://python-forum.io/thread-25206.html

## namedtuple 초기화하기 #anki
```python
from collections import namedtuple

DeliveryInfo = namedtuple(
    "DeliveryInfo",
    [
        "uuid",
        "accepted_dtm",
    ],
)

# 일반적인 초기화
DeliveryInfo(uuid="helo", accepted_dtm="world")
DeliveryInfo("helo", "world")


# list 로 초기화 하기
DeliveryInfo(["helo", "world"])

# dict로 초기화 하기
DeliveryInfo(**{"uuid":"helo","accepted_dtm":"world"})
```
ref: https://stackoverflow.com/questions/43921240/pythonic-way-to-convert-a-dictionary-into-namedtuple-or-another-hashable-dict-li
