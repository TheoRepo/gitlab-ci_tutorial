# 如何使用gitlab构建pipeline流水线

## 二、.gitlab-ci.yml的介绍与简单编写

学习视频
[GitLab CI/CD系列教程（三）：.gitlab-ci.yml的介绍与简单编写](https://www.bilibili.com/video/BV1kA411L7jS?spm_id_from=333.337.search-card.all.click&vd_source=8ba6ed28327bb7cef4adc064e3b342c1)

学习文档
[gitlab docs 中文](https://docs.gitlab.cn/jh/ci/yaml/index.html)
[gitlab docs 英文](https://docs.gitlab.com/ee/ci/yaml/index.html#onlyexcept-basic)

## 笔记
一、通过详细的视频解说入门
第一个视频: 如何在服务器上安装一个gitlab
第二个视频: gitlab runner的安装和注册
第三个视频: .gitlab-ci.yml文件具体内容的编写

**CICD的基本流程**
执行环境：gitlab runner（默认每一条流水线只有有一个任务并发执行，目的是为了减少内存的消耗）
执行内容：.gitlab-ci.yml
.gitlab-ci.yml放在项目的根目录下面
运行这个配置文件，就会生成一条流水线

**配置文件.gitlab-ci.yml要怎么编写**
[.gitlab-ci.yml 基本关键词的使用](https://blog.csdn.net/github_35631540/article/details/111029151)

常用关键词: script, after_script, allow_failure, artifacts, before_script, cache, coverage, dependencies, environment, except, extends, images, include, interruptible, only, pages, parallel, release, resource_group, retry, rules, services, stage, tags, timeout, trigger, variables, when

**三个概念**
pipeline就是流水线
stages定义流水线的阶段（如果没有执行stages，就是默认是test阶段）
- .pre
- build
- test
- deploy
- .post
job定义具体执行的任务（很多job是默认的）
script就是具体执行的脚本

**关键词精讲**
* stages
全局自定义阶段 

* script
shell脚本

* stage
任务内的阶段，必须从全局阶段中选

* retry
任务失败，自动取重试

* image
指定一个基础Docker镜像作为基础运行环境，经常用到的镜像有node java python docker

* tags
用于指定Runner, tags的取值范围是在该项目可见的runner tags中

* only/except
限定当前任务执行的条件
 - only 可以指定指定一个分支

* when 
when关键字是实现发生故障或尽管发生故障仍能运行的作业
 - when manual 手动执行

* cache
缓存是将当前工作环境目录中的一些文件，一些文件夹存储起来，用于在各个任务初始化的时候恢复


**搭建一条前端的ci/cd流水线**

如果自己想要研究流水线的话，可以在阿里云租一套服务器，然后按量收费，当自己需要做方案调研的时候，就访问那台服务器

任务列表
* 安装node包

* 编译

* docker镜像部署

使用的关键词有, image, stages, stage, tag, script, cache, docker build


```yaml
job_deploy:
    image: docker
    stage: deploy
    script: 
        - docker build -t folive .
        - if [ $(docker ps -aq --filter name=mylive-container) ]; then docker rm -f mylive-container;fi  # 如果在docker搜索到容器mylive-container, 就直接删除掉
        - docker run -d -p 8001:80 --name mylive-container folive
        - echo 'deploy docker image success. visit http://8.135.98.62:8001'
    when: manual
```

解决docker in docker的问题
```bash
"/usr/bin/docker:/usr/bin/docker", "/var/run/docker.sock:/var/run/docker.sock"
```

