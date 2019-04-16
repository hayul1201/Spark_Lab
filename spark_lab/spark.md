##script
```
##import SparkContext
dsrdd= sc.textFile("/loudacre/devicestatus.txt")

#position 19 first delimiter
dsarr1= dsrdd.map(lambda line: line.split(line[19]))

# exactly len value 14                              
dsarr2 = dsarr1.filter(lambda val : len(val) == 14)

# len value 14 에서 1,2,3,13,14 번째 
dsarr3 = dsarr2.map(lambda val: (val[0], val[1], val[2], val[12], val[13]))

# val[1] 데이터를 ' '로 분리하여 첫번째 값 확인(manufacture와 model name으로 분리)
dsarr4 = dsarr3.map(lambda val : val[1].split(' ')[0])

# 위 확인한 val[1] 데이터의 모델명만 값만 보이도록 처리
dsarr3_1 = dsarr2.map(lambda val: (val[0], val[1].split(' ')[0], val[2], val[12], val[13]))

#파일 저장
dsarr3_1.saveAsTextFile("/loudacre/devicestatus_etl")
```
##dsrdd.take(5)-rdd 데이터 구조 확인
```
u'2014-03-15:10:10:20,Sorrento F41L,8cc3b47e-bd01-4482-b500-28f2342679af,7,24,39,enabled,disabled,connected,55,67,12,33.6894754264,-117.543308253',
 u'2014-03-15:10:10:20|MeeToo 1.0|ef8c7564-0a1a-4650-a655-c8bbd5f8f943|0|31|63|70|39|27|enabled|enabled|enabled|37.4321088904|-121.485029632',
 u'2014-03-15:10:10:20|MeeToo 1.0|23eba027-b95a-4729-9a4b-a3cca51c5548|0|20|21|86|54|34|enabled|enabled|enabled|39.4378908349|-120.938978486',
 u'2014-03-15:10:10:20,Sorrento F41L,707daba1-5640-4d60-a6d9-1d6fa0645be0,8,22,60,enabled,enabled,disabled,68,91,17,39.3635186767,-119.400334708',
 u'2014-03-15:10:10:20,Ronin Novelty Note 1,db66fe81-aa55-43b4-9418-fc6e7a00f891,0,13,47,70,enabled,enabled,enabled,10,45,33.1913581092,-116.448242643']
```

##dsarr3_1.take(5)
```
[(u'2014-03-15:10:10:20',
  u'Sorrento',
  u'8cc3b47e-bd01-4482-b500-28f2342679af',
  u'33.6894754264',
  u'-117.543308253'),
 (u'2014-03-15:10:10:20',
  u'MeeToo',
  u'ef8c7564-0a1a-4650-a655-c8bbd5f8f943',
  u'37.4321088904',
  u'-121.485029632'),
 (u'2014-03-15:10:10:20',
  u'MeeToo',
  u'23eba027-b95a-4729-9a4b-a3cca51c5548',
  u'39.4378908349',
  u'-120.938978486'),
 (u'2014-03-15:10:10:20',
  u'Sorrento',
  u'707daba1-5640-4d60-a6d9-1d6fa0645be0',
  u'39.3635186767',
  u'-119.400334708'),
 (u'2014-03-15:10:10:20',
  u'Ronin',
  u'db66fe81-aa55-43b4-9418-fc6e7a00f891',
  u'33.1913581092',
  u'-116.448242643')]
```
