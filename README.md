```python
#爬系網與tip公告副程式
```


```python
import requests
from bs4 import BeautifulSoup
tip_temp={}
tip_old={}
tip_newS={}
itweb_old={}
itweb_temp={}
```


```python
tip_old.update({"1":"2"})
tip_old.update({"2":"2"})
tip_old.update({"3":"2"})
tip_old.update({"4":"2"})
tip_old.update({"5":"2"})
tip_old.update({"7":"2"})
tip_old.update({"8":"2"})
tip_old.update({"9":"2"})
tip_old.update({"10":"2"})
tip_old.update({"11":"2"})
print(tip_old)
len(tip_old)
```

    {'1': '2', '2': '2', '3': '2', '4': '2', '5': '2', '7': '2', '8': '2', '9': '2', '10': '2', '11': '2'}
    




    10




```python
def tip_update():
    global tip_old
    global tip_temp
    global tip_newS
    #http://www.takming.edu.tw/inetoffice/post/show.asp?key=81015
    r = requests.get("http://www.takming.edu.tw/inetoffice/post/ShowNews.asp")
    r.encoding='big5'
    soup = BeautifulSoup(r.text,"html.parser")
    sel = soup.select("a") 
    table = soup.find_all('table')
    for tr in table[1].find_all('tr')[3:]:
        ctemp = 0
        ctemp1 = ""
        ctemp2 = ""
        for td in tr.find_all('td'):
            ctemp+=1
            if ctemp==2:
                atd = td.select("a")
                for atdd in atd[:11]:
                    ctemp1=atdd["href"][13:]
                    #print(atdd["href"][13:])
            elif ctemp==3:
                ctemp2=td.text.split()[0]
                #print(td.text.split()[0])
        #print("-----")
        if len(tip_newS)<=10:
            tip_newS.update({ctemp1:ctemp2})
        else:
            break
    #print(tip_newS)
    tmp1={}
    tmp2={}
    for s in sel[:11]:
        if s['href'][13:]!='':
            tip_temp.update({s['href'][13:]:s.text})
            #print(s['href'][13:],s.text)
    #print(tip_temp)
    if len(tip_old)<10:
        tip_old = tip_temp.copy()
        return tmp1,tmp2
    else:
        tip_old_keys = list(tip_old.keys())
        #print(len(tip_old_keys))
        for i in list(tip_temp.keys()):
            if tip_old.get(i,"None")=="None":
                tmp1.update({i:tip_temp.get(i)})
                tmp2.update({i:tip_newS.get(i)})
        tip_old.clear()
        tip_old = tip_temp.copy()
        return tmp1,tmp2

    
def itweb_update():
    global itweb_old
    global itweb_temp
    r = requests.get("http://www.it.takming.edu.tw/")
    r.encoding='UTF8'
    soup = BeautifulSoup(r.text,"html.parser")
    sel = soup.select("#t2_table td a")
    sel2 = soup.select('#t2_table td')
    tmp3=[]
    tmp4=[]
    scount = 0
    for s in sel:
        #print(s['href'][4:].split('/')[0])
        tmp3.append(int(s['href'][4:].split('/')[0]))
    for s2 in sel2:
        scount+=1
        if scount%2==1:
            #print(s2.text)
            tmp4.append(s2.text)
    tmp5=dict(zip(tmp3, tmp4))
    tmp5_Max = max(tmp5, key=int)
    if len(itweb_old)<1:
        itweb_old.update({tmp5_Max:tmp5.get(tmp5_Max)})
        return 0,0
    else:
        itweb_old.clear()
        itweb_old.update({tmp5_Max:tmp5.get(tmp5_Max)})
        return tmp5_Max,tmp5.get(tmp5_Max)
    
    
    
```


```python
task1,task2 = tip_update() 
print(task1,task2)
```

    {} {}
    


```python
task3,task4 = itweb_update()
task3,task4
```




    (380, '108學年度科四甲乙實務專題(I)發表通過名單')




```python

```


```python

```
