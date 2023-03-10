# Calculator.js: node.js 演示项目

本应用使用node.js创建，并包含了试用mocha的单元测试代码。本应用的运行状态为网页中的计算器，如下图：

![](calculator-ui.png)

代码中使用node.js代码提供了REST APIs，其中提供各种数学计算功能单元。

使用mocah编写的测试代码可以完成所有以上API内部运算运算逻辑的验证，最终使用 `mocha-junit-reports` 来生成XML格式的测试结果文件以便 [Azure DevOps](https://azure.com/devops) 可以读取测试结果提供DevOps流水线的测试集成。

## 本地构建本项目的方式

1. 运行 `npm install` 安装所有依赖包
2. 运行 `npm test` 运行所有测试
3. 运行 `npm start` 启动应用，并打开 http://localhost:3000

使用Docker构建和运行项目的方式：

1. 复制 _demo-assets/demo-dockerfile 到根目录下的Dockerfile文件

2. 运行以下命令完成 `calculator` 容器构建打包

```shell
docker build -t calculator .
```

2. 运行 `calculator` 容器

```shell
docker run -itd -p 8080:3000 calculator
```

打开 http://localhost:8080 

应用启动后的状态如上图。

## SonaQube 运行

使用Docker容器的方式运行sonar-scanner-cli

将 sonar-project.properties 移动到根目录，然后运行一下命令

```shll
docker run \
    --rm \
    -e SONAR_HOST_URL="http://{SonaQubeServerUrl}" \
    -e SONAR_LOGIN="{Token}" \
    -v "${PWD}:/usr/src" \
    sonarsource/sonar-scanner-cli
```

## Run Postman Collection in Docker

使用PostMan编写和运行API测试

![](postman/postman.png)

```shell
# install newman to run postman collections in command line https://www.npmjs.com/package/newman
npm install -g newman
# install newman-reporter-junit to export test report in juntest format https://www.npmjs.com/package/newman-reporter-junitfull
npm install -g newman-reporter-junitfull

## newman sample command
newman run postman/boathouse-calculator.postman_collection.json -e postman/local-dev.postman_environment.json

## newman sample command with junit report export
newman run postman/boathouse-calculator.postman_collection.json -e postman/local-dev.postman_environment.json -r junitfull --reporter-junitfull-export './postman/result.xml' -n 2
```

```shell
## run newman using the newman docker container
docker run -v "${PWD}/postman:/etc/newman" -t postman/newman:alpine run boathouse-calculator.postman_collection.json -e local-dev.postman_environment.json
## with junit format report 
docker run -v "${PWD}/postman:/etc/newman" -t postman/newman:alpine run boathouse-calculator.postman_collection.json -e local-dev.postman_environment.json --reporters junit --reporter-junit-export 'result-docker.xml'
```



DEBUG

```shell

git clone https://github.com/zhouwenyang/boathouse-calculator
cd boathouse-calculator
docker run -it --init -p 3000:3000 --expose 3001 -p 3001:3001 -v "$(pwd):/home/project" theiaide/theia:next --inspect=0.0.0.0:3001
npm install 
运行debug

```shell
