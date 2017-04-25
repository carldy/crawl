# crawl
import urllib.request as request
import urllib.parse
import string
from bs4 import BeautifulSoup
def get_title(url):
    req=request.Request(url)
    with request.urlopen(req) as response: # with处理方式，还没有了解得很深入
        response=response.read().decode()
        stru_response=BeautifulSoup(response,"html.parser") #html.parse 这个是指明使用html分词方法进行分析，不指明的话会警告，但不会出错
        try:
            data=stru_response.table.findAll('tr') #findAll是BeautifulSoup的一个函数，可以找到所有tr这个标签内的文字，并返回一个list
            #tr是一行的意思， .table表示在response的第一个table里面找,这句话的意思就是在网页的第一个table里面把每一行都提取出来，并放到一个list里面
        except AttributeError as e:  #AttributeError是bs4里面标签不存在的一种错误，这个不存在指的是库中没有这个标签。
            return ('This attribute doesn\'t exist')
        else:
            return data
title_list=get_title('http://gaokao.xdf.cn/201702/10612921.html')
del title_list[0] #删掉两行表头
del title_list[0]
matrix=[[]for i in range(100)] #制作一个100*8的二维数据，但是为了防止浅拷贝，元素添加需要用append的方式
i=0
for item in title_list:
    item=str(item)
    item=item.replace('\n','')
    item=item.replace('\t','')
    item=item.strip()
    item=BeautifulSoup(item,'html.parser')
    item=item.findAll('p')
    for item in item:
        matrix[i].append(item.get_text()) #get_text()是一种bs4中的用法，是把标签都去掉，只剩下正文的内容
    i=i+1
#print(matrix)
for i in range(100):
    for j in range(8):
        print(matrix[i][j],end='\t') #这是一种让print不换行的方式，并且以'\t'结束一次输入
        if j==7:
            print()
    #item=item.replace('\t','')
