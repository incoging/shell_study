num = int(input().strip())
matrix = []
for _ in range(num):
    per = list(map(int, input().strip().split()))
    matrix.append(per)

flag = [[0] * num for _ in range(num)]


def search(content, i, j, flag):
    if 0<= i < num and 0<= j < num:
        if content[i][j] and not flag[i][j]:
            flag[i][j] = True
            search(content, i-1, j, flag)
            search(content, i+1, j, flag)
            search(content, i, j-1, flag)
            search(content, i, j+1, flag)
            return True
    return False

def num_of_connect(content):
    count = 0
    for i in range(num):
        for j in range(num):
            if search(content,i,j, flag):
                count += 1
    print(count)

num_of_connect(matrix)

