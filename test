line = list(input().strip("\n"))
num= 1
pre= 0
for i in range(len(line) -1):
    if line[i] == line[i+1]:
        if (line[i] == line[0] and line[i+1] == line[-1]) or (line[0] == line[-1]):
            if pre <num:
                pre = num
            num =1
            continue
        else:
            part1 = line[:i+1]
            reversed(part1)
            part2 = line[i+1:]
            reversed(part2)
            line = part1 + part2
            num += 1
    else:
        num +=1

print(num)
