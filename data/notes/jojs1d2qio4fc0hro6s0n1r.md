
# Indeed

分为两部分，原因是反爬虫机制比较强

## search page

通过indeed job search api 获取数据，存在sqlite中

## get detail info

获取每个job的详细信息，存在sqlite中

## sqlite column

```
Table("indeed", metadata,
          Column('Id', String, primary_key=True, nullable=False),
          Column('Date', Date), Column('Company', String),
          Column('Title', String), Column('Review', Float),
          Column('Experience', String),
          Column('Citizen', Boolean),
          Column('Type', String),
          Column('H1B', String), Column('Fulltime', String),
          Column('City', String), Column('State', String),
          Column('Link', String),
          Column('Clicked', Integer),
          Column('Liked', Integer),
          Column('Disliked', Integer),
          Column('ApplyLink', String),
          Column('Expire', Boolean),
          Column('TestCount', Integer),
          Index('date_index', 'Date'), Index('state_index', 'State'))

```