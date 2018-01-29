## Docker 

### DockerFile
- Python镜像文件
```
FROM python:2.7
ENV PYTHONUNBUFFERED 1

RUN mkdir /usr/src/app
WORKDIR /usr/src/app
ADD requirements /usr/src/app
RUN pip install -r requirements
ADD . /usr/src/app
RUN python setup.py install
```

### 生成Images
- docker build -t python:0.1 .

### Pycharm配置
- 打开PyCharm -> Project Interpreter -> Add Remote -> 