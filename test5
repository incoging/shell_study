N = int(input().strip())
M = int(input().strip())

my_input = list(map(int, input().strip().split()))

my_map = {}
for i in set(my_input):
    my_map[i] = set()

i = 0
while i < len(my_input):
    start, end = my_input[i : i + 2]
    my_map[end].add(start)
    for it in my_map[start]:
        my_map[end].add(it)
    i += 2
    pass

ans = 0
for key, value in my_map.items():
    count = 0
    for key2, value2 in my_map.items():
        if key == key2:
            continue
        if key in value2:
            count += 1
    if count == N - 1:
        ans = key
        break


print(ans)



