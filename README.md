# 如何使用gitlab构建pipeline流水线

学习视频
![GitLab CI/CD系列教程（三）：.gitlab-ci.yml的介绍与简单编写](https://www.bilibili.com/video/BV1kA411L7jS?spm_id_from=333.337.search-card.all.click&vd_source=8ba6ed28327bb7cef4adc064e3b342c1)

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
![.gitlab-ci.yml 基本关键词的使用](https://blog.csdn.net/github_35631540/article/details/111029151)
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

stages  # 全局自定义阶段 
script  # shell脚本
stage   # 任务内的阶段，必须从全局阶段中选
retry   # 任务失败，自动取重试
    max
    when
        - runner_system_failure
        stuck_or_timeout_failure
only    # 