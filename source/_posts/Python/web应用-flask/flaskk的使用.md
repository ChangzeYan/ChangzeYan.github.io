---
title: flaskk的使用
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

安装：
```bash
pip install graphene flask-graphql
```

然后编写query 和schema：
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
    def resolve_hello(root, info, name):
        return f'Hello: {name} !'

    def resolve_goodbye(root, info):
        return Result(1, "success")


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
    app.run()

```

运行：http://127.0.0.1:5000/graphql

![graphiql测试](https://github.com/ChangzeYan/ChangzeYan.github.io/raw/hexo/source/pic/flask-graphql-运行.png)
