# Aggregation

> https://docs.mongodb.com/manual/meta/aggregation-quick-reference/#aggregation-expressions

- MongoDB 의 Aggregation 은 Sharding 기반의 데이터를 효율적으로 처리하고 집계하는 프레임워크
- documents 를 grouping, filtering 등 다양한 연산을 적용하여 계산된 결과를 반환
  - 주요 mongodb aggregation operators:
    - Ex. filtering, like operation, transforming
- document 를 여러 단계의 파이프라인으로 처리해서, 데이터를 처리/집계하는 역할

![agg](https://user-images.githubusercontent.com/47518272/159279792-08efbf79-24bc-4e60-8242-f04e4735bc3f.png)

## SQL 과 비교

![sqlmongo](https://user-images.githubusercontent.com/47518272/159279910-12473172-1517-4d5c-b034-811529ef4267.png)

## $group

SQL 구문의 GROUP BY 와 유사한 역할을 수행한다.

### 기본 형태

```
{
  $group:
    {
      _id: <expression>, // Group By Expression
      <field1>: { <accumulator1> : <expression1> },
      ...
    }
 }
```

accumulator에 필요한 연산을 활용하면 특정 필드에 연산을 수행한 값이 들어가게하고 그룹화 할 수 있다.

### 연산 종류

![group](https://user-images.githubusercontent.com/47518272/159280302-642b0140-121b-4902-a196-b7bf520d5fbf.png)

- action 컬럼으로 그룹핑

```sql
db.getCollection('smslogs').aggregate([{
    $group: {
        _id: "$action",
        count: {$sum:1}
    }
}])
```

- id 컬럼으로 그룹핑

```sql
db.sales.aggregate([
  {
    $group: {
       _id: null,
       count: { $sum: 1 }
    }
  }
])
```
```
{ "_id" : null, "count" : 8 }
= SELECT COUNT(*) AS count FROM sales
```

좀 더 복잡한 예시로

item 필드와 전체판매양(totalSaleAmount)에 대한 값을 그룹핑한 후 totalSaleAmount이 100 이상인 경우에 대해서만 출력하게 한다면,

아래와 같이 표현할 수 있다.

```sql
db.sales.aggregate(
  [
    // First Stage
    {
      $group :
        {
          _id : "$item",
          totalSaleAmount: { $sum: { $multiply: [ "$price", "$quantity" ] } }
        }
     },
     // Second Stage
     {
       $match: { "totalSaleAmount": { $gte: 100 } }
     }
   ]
 )
```
```
{ "_id" : "abc", "totalSaleAmount" : NumberDecimal("170") }
{ "_id" : "xyz", "totalSaleAmount" : NumberDecimal("150") }
{ "_id" : "def", "totalSaleAmount" : NumberDecimal("112.5") }
```

## $match

위의 group 예시에서도 사용되었듯이 $match는 특정 조건에 일치하는 값만 반환하도록 할 때 사용한다.

### 기본 형태

```
{ $match: { <query> } }
```

author 가 dave인 도큐먼트만 출력한다면 아래와 같이 명령할 수 있다.

```sql
db.articles.aggregate(
    [ { $match : { author : "dave" } } ]
);
```

## $sort

정렬을 위해 사용한다.

### 기본 형태

```
{ $sort: { <field1>: <sort order>, <field2>: <sort order> ... } }
```

정렬을 원하는 필드에 대한 오름차순이라면 `1`, 내림차순이라면 `-1` 이라고 지정해주면 된다.

```sql
db.users.aggregate(
   [
     { $sort : { age : -1, posts: 1 } }
   ]
)
```
