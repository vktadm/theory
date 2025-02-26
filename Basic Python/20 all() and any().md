```python
a = [True, True, True, True]
b = all(a) # True

a = [True, False, True, True]
b = all(a) # False

a = [{}, 1, True, [3, 4], 'str']
b = all(a) # False
```

```python
a = [{}, 1, True, [3, 4], 'str']
b = any(a) # True
```