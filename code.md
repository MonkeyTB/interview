## 两个list关联排序

```python
# 打包
zipped = zip(sen,count)
# 排序
sort_zipped = sorted(zipped,key=lambda x:(x[1],x[0]),reverse=True)
# 还原
result = zip(*sort_zipped)
sen,count = [list(x) for x in result]
```

## pandas 行列显示不全解决方式

```python
#显示所有列
pd.set_option('display.max_columns', None)
#显示所有行
pd.set_option('display.max_rows', None)
#设置value的显示长度为100，默认为50
pd.set_option('max_colwidth',100)
```

## git

```reStructuredText
# 撤回上一次的commit
git reset --soft HEAD^
# 删除远程分支
git push origin --delete branch-zlf
# 删除本地分支
git branch -d branch-zlf
# 创建分支
git branch branch-zlf
# 切换分支
git checkout branch-zlf
```

## Tensor Flow Addons 版本问题

| --                       | --         | --              |
| ------------------------ | ---------- | --------------- |
| Tensor Flow Addons       | TensorFlow | Python          |
| tfa-nightly              | 2.2        | 3.5,3.6,3.7,3.8 |
| twnsorflow-addons-0.10.0 | 2.2        | 3.5,3.6,3.7,3.8 |
| tensorflow-addons-0.9.1  | 2.1,2.2    | 3.5,3.6,3.7     |
| tensorflow-addons-0.8.3  | 2.1        | 3.5,3.6,3.7     |
| tensorflow-addons-0.7.1  | 2.1        | 2.7,3.5,3.6,3.7 |
| tensorflow-addons-0.6.0  | 2.0        | 2.7,3.5,3.6,3.7 |

## pandas 删除Nan值

```python
# dataframe 行至少有两个不为 Nan 保留,inplace True 原dataframe上进行操作
df_mark.dropna(how = 'all',thresh = 2,inplace=True)
# dataframe 行全为 Nan 删除
df_mark.dropna(how = 'all',inplace=True)
# dataframe 行有一个 Nan 删除
df_mark.dropna(how = 'any',inplace=True)
```

## 字符串查找首次出现的位置

```python
import re
content = '1、中专以上学历，农业相关专业，从事农业技术应用与实践工作一年以上； 2、热爱农业，为人踏实稳重，能吃苦耐劳，工作认真细致，有责任感，团队协作意识强。'
i = '农业'
re.search(re.compile(i),content).span()
# output 
(9, 11)
```

## pandas 一行切分变多行

```python
df_jd_content_split = df_jd_content['detail'].str.split('<br>|。|；\d、',expand=True).stack()
df_jd_content_split = df_jd_content_split.reset_index(level=1,drop=True).rename('detail')
df_jd_content_split = pd.DataFrame(df_jd_content_split)
```

## pyhanlp 依存句法分析结果解析

```python
In [56]: print(HanLP.parseDependency('宽带微波收发电路设计'))                                                                                       
1       宽带微波        宽带微波        nh      nr      _       2       主谓关系        _       _
2       收发    收发    v       v       _       0       核心关系        _       _
3       电路设计        电路设计        nz      nz      _       2       动宾关系        _       _
In [62]: for word in HanLP.parseDependency('宽带微波收发电路设计'): 
    ...:     print(word.ID) 
    ...:     print(word.LEMMA) 
    ...:                                                                                                                                            
1
宽带微波
2
收发
3
电路设计
```

```java
/**
 * ID  当前词在句子中的序号，１开始.
 */
public int ID;
/**
 * 当前词语（或标点）的原型或词干，在中文中，此列与FORM相同
 */
public String LEMMA;
/**
 * 当前词语的词性（粗粒度）
 */
public String CPOSTAG;
/**
 * 当前词语的词性（细粒度）
 */
public String POSTAG;
/**
 * 当前词语的中心词
 */
public CoNLLWord HEAD;
/**
 * 当前词语与中心词的依存关系
 */
public String DEPREL;

/**
 * 等效字符串
 */
public String NAME;

sb.append(ID).append('\t').append(LEMMA).append('\t').append(LEMMA).append('\t').append(CPOSTAG).append('\t') .append(POSTAG).append('\t').append('_').append('\t').append(HEAD.ID).append('\t').append(DEPREL).append('\t').append('_').append('\t').append('_');

sb.append(ID).append('\t').append(LEMMA).append('\t').append(LEMMA).append('\t').append(CPOSTAG).append('\t').append(POSTAG).append('\t').append('_').append('\t').append(HEAD.ID).append('\t').append(DEPREL).append('\t').append('_').append('\t').append('_');
```

## pandas 某列 按某个值删除

```python
# content：列名，''：可以换成其他的，None等
df_search_space = df_search_space[~df_search_space.content.isin([''])]
```

## python 判断字符串异常

```python
def judge(hostip):
    if hostip is None or not hostip or hostip == '' or hostip == 'null':
        return False
    else:
        return True
```

## sql group by 每个类随机抽取

```sql
select
	*
from
  (
    select
      *,
      row_number() over(
        partition by job_init_title
        order by
          rand()
      ) as rn
    from
      dw_chihiro_algo_d_job_feature
    where
      p_date == '20210419'
  ) a
where
  a.rn <= 500
  
-- https://blog.csdn.net/qq_29232943/article/details/104635492
```

## pandas 增加两列

```python
def cut_match(x):
    num = 0 
    words = [i for i in jieba.cut(x.job_title) if len(i) != 1]
    for word in words:
        if word in x.content:
            num += 1
    return (num,len(words))
df_match[['contain','total']] = df_match.apply(lambda x:cut_match(x),axis=1,result_type='expand')
# result_type='expand' 必须加
```

## pandas 转 dict【行】

```python
import pandas as pd
df = pd.DataFrame({"姓名": ["古明地觉"],
                   "水果": ["草莓"],
                   "星期一": ["70斤"],
                   "星期二": ["72斤"],
                   "星期三": ["60斤"],
                   })
out:
姓名	水果	星期一	星期二	星期三
0	古明地觉	草莓	70斤	72斤	60斤

pd.melt(df, value_vars=df.columns)
out:
    variable	value
0	姓名	古明地觉
1	水果	草莓
2	星期一	70斤
3	星期二	72斤
4	星期三	60斤

pd.melt(df, value_vars=df.columns).set_index('variable')['value'].to_dict()
out:
    {'姓名': '古明地觉', '水果': '草莓', '星期一': '70斤', '星期二': '72斤', '星期三': '60斤'}
```

## pb模型打印详细信息

```python
saved_model_cli show --dir saved_model/1/ --all
```

