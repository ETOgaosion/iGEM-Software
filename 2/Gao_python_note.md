sys.exit();
elif
None=null
print自动加\n
print(xx,end='')end替换结尾\n
print(xx,xx,sep='')sep替换间隔字符串
全局变量前加global
\续行符，下一行不需注意缩进


负数下标：循环排列%len(list)
list:使用[]，有下标，有切片
切片[包括:不包括]
del listname[i]，删除列表下标处的值并且之后的项往前移动
list增加元素：listname+[需增加的元素]
遍历列表：for i in range(len(listname))
range(i):从0到i-1
range(x,y):从x到y-1
查找元素：in, not in
将列表元素分别赋值到各个元素：a,b,c=listname（list有3元素）
必须严格相等
列表可以+=,*=(将列表原顺序放在最后)
增加值也可以用listname.append(加入元素)
插入值listname.insert(index,插入元素)原下标处元素及后面元素往后移动
删除值：listname.remove(删除元素，非下标)从前往后搜索删除
排序：listname.sort()字典序，不用listname=listname.sort()
listname.sort(key=xx)
列表可变，但字符串不可变

元组：('xx','xx',xx,x.x)
不同数据类型
同样有下标、切片
元组数据不可变
赋值时元组只有一个值应在后面加,

list(),tuple()转换类型

元素互相赋值时值传递，开辟新内存
列表互相赋值时内存传递
函数修改列表时不用返回值，可直接对列表操作

copy模块，list互相赋值时开辟新内存
list1name=copy.copy(list2name)
列表中包含列表，使用deepcopy()

字典：键值对，使用花括号：cat={'size':xx,'xx':xx}
可通过键名访问，键名可以直接为数字（不必带引号）
字典本身不排序，无切片
字典方法：keys(),values(),items()对应键，值，键值对，数据类型为：dict_keys,dict_values,dict_items
for k in cat.keys():
for i in cat.items():
for v in cat.values():
可以list(cat.keys())
查找：'color' in spam='color' in spam.keys()
'xx' in spam.value()
get()方法：spam.get('xx'.0)，前者是键名，后者是默认值，若无键则返回默认值
设置默认键值对：spam.setdefault('color',默认值)
setdefault可以用作设置spam，向其中加入键值对

漂亮打印：
import pprint
pprint.pptint(xx)可将键排序后打印，多用于包含嵌套的列表和字典
pprint.pformat(xx)漂亮变为字符串

字符串头尾"和'无所谓
转义字符\
字符串打印时前面加r：print(r'xxx')，打印原始字符串
三重引号，多行打印
单行注释#，多行注释：
```python
"""

"""
```
字符串自动赋予下标，有切片
'xx' in 'xxx'查找是否包含，区分大小写，not in类似
字符串newspam=spam.upper()
newspam=spam.lower()转换大小写
spam.islower()，spam.isupper()判断是否全部大小写
str.startswith('xxx')
str.endwith('xxx')判断是否以某字符串开始和结束
','.join(['zzz','xxx'])以,分割，传入列表，结果返回字符串：'zzz,xxx'
str.split()按照空白字符分割，输出列表
str.rjust(10)右对齐，字符串长度为10，前加空格输出字符串
str.tjust(10,'*')前面填充*
ljust()类似
center()类似
spam.strip()删除头尾空格
spam.lstrip(),spam.rstrip()删除左，右空格，字符串长度不变，在右，左填充空格
spam.strip('xxx')字符串中删除xxx从两边开始删，直到不是xxx
pyperclip模块拷贝粘贴字符串，需要安装
pyperclip.copy('xxx')xxx复制到剪贴板
pyperclip.paste()打印剪贴板上的内容，若剪贴板更新了内容，打印新内容

正则表达式模块：re
\d数字
object=re.compile(r'\d\d')object
mo=object.search('xxx')，mo为match对象，mo.group()字符串
re.compile(r'('\d\d)('\d\d\d\d'))以括号分割后mo.group(i)返回第i个满足的括号内容mo.group(0)=mo.group()
r'xxx|xxx或
?前面的()分组是可选的
*前面的分组可以出现0次或多次
+前面分组出现1次或多次
{3}前面分组出现3次
{x,y}前面分组出现x或x+1或...或y次（都包括），默认为贪心的，匹配最长的字符串，
{x,y}?非贪心，匹配最短字符串
object.findall('xxx')返回一个字符串，匹配所有满足条件的表达式
缩写字符分类：
\d 0-9
\D 除了0-9以外的所有字符
\w 任何字母、数字或下划线字符（可以认为是匹配“单词”字符）
\W 除字母、数字和下划线以外的任何字符
\s 空格、制表符或换行符（可以认为是匹配“空白”字符）
\S 除空格、制表符和换行符以外的任何字符
[]方括号自定义字符分类，匹配方括号内任意字符
[^]匹配不在方括号内的任意字符
object=re.compile(r'[aeiouAEIOU])
^以以下字符串开始
$以上一个匹配字符串结尾
.通配符，匹配所有换行符之外的单个字符
.*匹配所有字符串（除换行？），任意文本，任意长字符串
re.compile('.*',re.DOTALL)能够匹配换行符
re.compile(r'xxx',re.I)忽略大小写
object.sub('xxx','xxxxx')替换所有匹配的字符串

file:
open()打开文件
read()文件内容写入变量
xx=open('xx.txt','w')写入模式打开
xx.write('aac')写入
xx.close()

```python
    import requests, bs4
    res = requests.get('http://nostarch.com')
    res.raise_for_status()
    noStarchSoup = bs4.BeautifulSoup(res.text)
    type(noStarchSoup)
```
<class 'bs4.BeautifulSoup'>
requests.get()函数从No Starch Press 网站下载主页，然后将响应结果的text 属性传递给bs4.BeautifulSoup()

连续下载漫画代码：
```python
import requests
import os
import bs4

url = 'http://xkcd.com'
os.makedirs('cartoons', exist_ok=True)
while not url.endswith('#'):
    # download webpage
    print('Downloading the page %s...' % url)
    res = requests.get(url)
    res.raise_for_status()
    soup = bs4.BeautifulSoup(res.text, "html.parser")
    # look for cartoon
    comic = soup.select('div[id="comic"]>img')
    if comic == []:
        print('Couldn\'t find any cartoon image.')
    else:
        comicurl = 'http:' + str(comic[0].get('src'))
        print('comic url:%s' % comicurl)
        res = requests.get(comicurl)
        res.raise_for_status()
        # Save image
        imagefile = open(
            os.path.join('D:\\pycode\\vscode\\cartoons',
                         os.path.basename(comicurl)), 'wb')
        for chunk in res.iter_content(100000):
            imagefile.write(chunk)
        imagefile.close()
    # Get the Prev button
    prevlink = soup.select('a[rel="prev"]')[0]
    url = 'http://xkcd.com' + prevlink.get('href')

print('Done!')
```
读取excel文件代码：
```python
import openpyxl
import os
import pprint

print("Opening sheet...")
wb = openpyxl.load_workbook(
    os.path.join('automate_online-materials', 'censuspopdata.xlsx'))
sheet = wb.get_sheet_by_name("Population by Census Tract")
countydata = {}

print("Reading data...")
for row in range(2, sheet.get_last_row() + 1):
    state = sheet['B' + str(row)].value
    county = sheet['C' + str(row)].value
    pop = sheet['D' + str(row)].value
    countydata.setdefault(state, {})
    countydata[state].setdefault(county, {'tracts': 0, 'pop': 0})
    countydata[state][county]['tracts'] += 1
    countydata[state][county]['pop'] += int(pop)

# Open a new text file and write the contents of countyData to it.
print('Writing results...')
resultFile = open('census2010.py', 'w')
resultFile.write('allData = ' + pprint.pformat(countydata))
resultFile.close()
print('Done.')
```