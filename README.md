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

or even cooler one:

```python
import curses, random, time

CHARS = "01abcdefghijklmnopqrstuvwxyz$#@%&*+=<>"

def main(stdscr):
    curses.curs_set(0)
    stdscr.nodelay(True)
    curses.use_default_colors()
    curses.init_pair(1, curses.COLOR_GREEN, -1)
    green = curses.color_pair(1)

    h, w = stdscr.getmaxyx()
    drops = [[random.randint(-h, 0), random.randint(5, 15)] for _ in range(w)]

    while stdscr.getch() != ord("q"):
        for x, (y, n) in enumerate(drops):
            try:
                if 0 <= y < h:  # bright head
                    stdscr.addch(y, x, random.choice(CHARS), green | curses.A_BOLD)
                if 0 <= y - n < h:  # erase tail
                    stdscr.addch(y - n, x, " ")
            except curses.error:
                pass
            drops[x][0] += 1
            if drops[x][0] - n > h:  # respawn at top
                drops[x] = [random.randint(-10, 0), random.randint(5, 15)]
        stdscr.refresh()
        time.sleep(0.05)

curses.wrapper(main)
```

