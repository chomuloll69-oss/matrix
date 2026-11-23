```python
import random, time

w = 80
on = [False] * w
print("\033[32m")  # green 💚

while True:
    on = [not a if random.random() < 0.08 else a for a in on]
    print("".join(random.choice("01") if a else " " for a in on))
    time.sleep(0.05)
```
