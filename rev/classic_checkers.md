Wrote a Python Script to do the z3 solving:

```python
from z3 import *

flag = IntVector("flag", 34)


s = Solver()

s.add(flag[0] == ord("U"))
s.add(flag[1] == ord("D"))
s.add(flag[2] == ord("C"))
s.add(flag[3] == ord("T"))
s.add(flag[4] == ord("F"))
s.add(flag[5] == ord("{"))

s.add(flag[6] == 104)

s.add(flag[9] + 15 == flag[8] - flag[7])

s.add(flag[7] * flag[9] == 2652)

s.add(flag[7] - flag[9] == 1)

s.add(flag[10] == flag[14])

s.add(flag[14] == flag[21])

s.add(flag[10] == flag[25])

s.add(flag[21] == flag[27])

s.add(flag[10] == flag[14])

s.add((flag[10] / 2) + 1 == flag[12])

s.add(952 == flag[11] ** 2 - flag[13] ** 2)

s.add(flag[14] == ord("_"))
s.add(flag[15] == ord("a"))
s.add(flag[16] == ord("l"))
s.add(flag[17] == ord("w"))
s.add(flag[18] == ord("4"))
s.add(flag[19] == ord("y"))
s.add(flag[20] == ord("s"))

s.add(flag[22] == flag[6])
s.add(flag[23] == flag[7])

s.add(flag[24] % 97 == 3)

s.add(flag[14] == flag[27])
s.add(flag[15] == flag[26])

s.add(flag[28] == 76)
s.add(flag[29] == 49)

v1 = Int("v1")
v2 = Int("v2")
s.add(flag[30] * v1 == 6640)
s.add(flag[31] * v2 == 6640)
s.add(flag[30] != flag[31])

s.add(flag[30] > flag[31])
s.add(flag[32] == flag[31] + flag[30] - flag[24])
s.add(flag[33] == 125)

# Enforce ASCII
for i in range(34):
    s.add(flag[i] >= 33, flag[i] < 127)

print("SAT:", s.check())
model = s.model()
for i in range(34):
    print(chr(model[flag[i]].as_long()), end="")

print()
```

Output: `UDCTF{h4v3_y0u_alw4ys_h4d_a_L1SP?}`

Coding was heavily supported by ChatGPT.
