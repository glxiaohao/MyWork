# 3.1 python
![](../Images/26.jpg)
![](../Images/27.jpg)
~~~python
#coding:utf-8
'''
python 快速排序
'''
if __name__ == '__main__':
    l = [5,1,3,8,9,4,7,6]
    s="-"
    def first_sort(l):
        flag = l[-1]
        i = -1
        for j in range(len(l)-1):
            if l[j] > flag:
                pass
            else:
                i += 1
                tmp = l[i]
                l[i] = l[j]
                l[j] = tmp
        print(l)
        print("\n" + "*"*20 + "\n")

        tmp = l[-1]
        l[-1] = l[i+1]
        l[i+1] = tmp

        print(l)
        print("\n" + "*"*20 + "\n")

    first_sort(l)

if __name__ == '__main__':
    l = [5,1,3,8,9,4,7,6]

    def first_sort(l):
        flag = l[-1]
        i = -1
        for j in range(len(l)-1):
            if l[j] > flag:
                pass
            else:
                i += 1
                tmp = l[i]
                l[i] = l[j]
                l[j] = tmp
        print(l)
        print("\n" + "*"*20 + "\n")

        tmp = l[-1]
        l[-1] = l[i+1]
        l[i+1] = tmp

        print(l)
        print("\n" + "*"*20 + "\n")

    first_sort(l)

if __name__ == '__main__':
    l = [5,1,3,8,9,4,7,6]

    def first_sort(l):
        flag = l[-1]
        i = -1
        for j in range(len(l)-1):
            if l[j] > flag:
                pass
            else:
                i += 1
                tmp = l[i]
                l[i] = l[j]
                l[j] = tmp
        print(l)
        print("\n" + "*"*20 + "\n")

        tmp = l[-1]
        l[-1] = l[i+1]
        l[i+1] = tmp

        print(l)
        print("\n" + "*"*20 + "\n")

    first_sort(l)


if __name__ == '__main__':
    l = [5,1,3,8,9,4,7,6]

    def first_sort(l):
        flag = l[-1]
        i = -1
        for j in range(len(l)-1):
            if l[j] > flag:
                pass
            else:
                i += 1
                tmp = l[i]
                l[i] = l[j]
                l[j] = tmp
        print(l)
        print("\n" + "*"*20 + "\n")

        tmp = l[-1]
        l[-1] = l[i+1]
        l[i+1] = tmp

        print(l)
        print("\n" + "*"*20 + "\n")

    first_sort(l)

if __name__ == '__main__':
    l = [5,1,3,8,9,4,7,6]

    def first_sort(l):
        flag = l[-1]
        i = -1
        for j in range(len(l)-1):
            if l[j] > flag:
                pass
            else:
                i += 1
                tmp = l[i]
                l[i] = l[j]
                l[j] = tmp
        print(l)
        print("\n" + "*"*20 + "\n")

        tmp = l[-1]
        l[-1] = l[i+1]
        l[i+1] = tmp

        print(l)
        print("\n" + "*"*20 + "\n")

    first_sort(l)
~~~