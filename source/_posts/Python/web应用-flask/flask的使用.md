---
title: flask的使用
author: ChangzeYan
date: 2020-12-15 22:21:25
tags: Flask
categories: Python
cover:
---

# 安装flask
```bash
pip install flask
```
其他可能用到的包（非必须）：
```bash
pip install sqlalchemy graphene flask-graphql flask-sqlalchemy graphene-sqlalchemy
# MySQL
pip install mysqlclient
# PostgreSQL
pip install psycopg2-binary
```

# hello world
```py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```
打开浏览器访问http://127.0.0.1:5000/，浏览页面上将出现Hello World!。


# Flask结合graphql

## 安装：
```bash
pip install graphene flask-graphql
```

## 然后编写query 和schema：
```py
from flask import Flask
from graphene import ObjectType, String, Schema, Int,Field
from flask_graphql import GraphQLView


class User(ObjectType):
    id = Int()
    name = String()


class Result(ObjectType):
    code = Int()
    msg = String()


class Query(ObjectType):
    # this defines a Field `hello` in our Schema with a single Argument `name`
    hello = String(name=String(default_value="stranger"))
    goodbye = Field(Result)

    # our Resolver method takes the GraphQL context (root, info) as well as
    # Argument (name) for the Field and returns data for the query Response
    # resolve需要加上固定前缀resolve_
    def resolve_hello(root, info, name):
        return f'Hello: {name} !'

    def resolve_goodbye(root, info):
        return Result(1, "success")

    users = List(User, id=Int(required=True))
    user = Field(User, id=Int(required=True))


    def resolve_user(self, info, id):
        """返回单个实例对象"""
        return User(id=id, name='zhangsan')


    def resolve_users(self, info, id):
        """返回列表对象"""
        return [User(id=id, name='lisi')]



schema = Schema(query=Query)


# app = Flask(__name__)
# app.add_url_rule('/graphql', view_func=GraphQLView.as_view('graphql', schema=schema, graphiql=True))


def create_app():
    app = Flask(__name__)
    app.add_url_rule('/graphql', view_func=GraphQLView.as_view('graphql', schema=schema, graphiql=True))

    # Optional, for adding batch query support (used in Apollo-Client)
    # app.add_url_rule('/graphql/batch', view_func=GraphQLView.as_view('graphql', schema=schema, batch=True))

    @app.route("/")
    def hello_world():
        return "Hello World!"

    return app


if __name__ == '__main__':
    app = create_app()
    # 默认端口号5000
    app.run()
    # 指定端口号5001
    # app.run(port=5001)

```

## 运行
打开：http://127.0.0.1:5000/graphql

![graphiql测试](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/flask-graphql-运行.png)

## 参数传递和获取
参考：
  - [详解Python Graphql](https://www.cnblogs.com/gaojy/p/11678191.html)
  - [Python使用graphene-sqlalchemy提供GraphQL接口](https://haofly.net/python-graphql/)


```py
class NER_RESULT(ObjectType):
    word_list = List(String)


class Query(ObjectType):
    # 返回值，参数
    ner = Field(NER_RESULT, text=String())

    # 设置传入参数列表，args
    def resolve_ner(self, info, **args):
        text = args['text']
        print(text)
        words = word_tokenize(text)
        print(words)
        key_words = [word.strip() for word in open("key_words.txt", "r", encoding="utf-8").readlines()]
        word_list = []
        ner_result = NER_RESULT(word_list)
        return ner_result
```

访问：
```bash
query{
 	ner(text:"When Jobs arrived back at Apple, it had a conventional structure for a company of its size and scope."){
    wordList
  }
}
```

### info参数
info表示请求的上下文，可以在查询语中添加context：
```py
class Query(ObjectType):
     hello = String(name=String(default_value="gaojy", required=True))
     @staticmethod
     def resolve_hello(root, info, name):
        # 通过info可获取上下文内容
        print(info.context.get('company'))
        return f"hello word -- {name}"


schema = Schema(query=Query, mutation=MyMutations)

if __name__ == '__main__':
    query_string = '''{ hello(name:"gaojiayi") }'''
    # 1 execute中添加context
    result = schema.execute(query_string, context={'company': 'baidu'})
    print(result.data['hello'])
```

## 返回值
### 返回基本类型
```py
class Query(graphene.ObjectType):
    add = graphene.Int(
        description='calculate a + b then return the result.',
        a=graphene.Int(),
        b=graphene.Int())

    @staticmethod
    def resolve_add(obj, info, a=0, b=0, **kwargs):
        return a + b
```
请求：
```bash
requests.post('http://localhost:5000/graphql', json={
    'query': '{add(a: 4, b: 5)}'
})
```
### 返回列表
```py
class Query(graphene.ObjectType):
    rand = graphene.List(
        graphene.Int,
        description='get some random numbers from 0 to 100',
        count=graphene.Int(),
    )

    @staticmethod
    def resolve_rand(obj, info, count=1, **kwargs):
        return [random.randint(0, 100) for i in range(count)]
```
