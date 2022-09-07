
# usage
- for loop로 장황하게 사용하고 싶지 않거나 functional concept 함수와 lambda를 사용하고 싶지 않은 경우 사용.


```python
animals = ["eagle", "fox", "frog", "tiger"]

results = [x for x in animals if x.startsWith("f") ]
```

	