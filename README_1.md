# 如何使用gitlab构建pipeline流水线

## 一、Docker安装GitLab Runner

学习视频
![GitLab CI/CD系列教程（二）：Docker安装GitLab Runner](https://www.bilibili.com/video/BV185411c7bJ/?spm_id_from=333.788&vd_source=8ba6ed28327bb7cef4adc064e3b342c1)

## 概念

CICD做了一件什么事情？
**一个理念**
当你把本地的代码推送到gitlab上面的时候，它会自动触发一个流程，这个流程会执行一系列的任务，着一系列的任务组装起来就是一个pipelines（一些列的任务按照一定的顺序运行）

gitlab runner是什么？
**一个基础环境**
gitlab只是一个代码管理平台
但是CICD的任务是在哪里运行的呢？—— gitlab runner：给流水线提供了运行环境
- 下载依赖包
- 项目编译
- 项目发布
- 项目部署

.gitlab-ci.yml是什么？
**一个配置文件**
- 定义的内容就是流水线的内容
- 一个yml文件，采用缩进的方式去编写

## 实操
1.  安装gitlab-runner（在远程服务器上运行）
```bash
sudo docker run -d --name gitlab-runner --restart always \
    -v /srv/gitlab-runner/config:etc/gitlab-runner \
	-v /var/run/docker.sock:/var/run/docker.sock \
	gitlab/gitlab-runner:lastest
```
参数解释: 
`--restart always`
- 重启方式，宕机了以后会自动重启
`gitlab/gitlab-runner:lastest`
- 镜像: 容器
`-v /srv/gitlab-runner/config:etc/gitlab-runner`
- gitlab-runner的目录
`-v /var/run/docker.sock:/var/run/docker.sock`
- docker的目录

2. 注册gitlab runner
使gitlab和gitlab runner产生联系
在gitlab runner中生成一个容器，指向我们的gitlab

```bash
docker run --rm -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register \
    --non-interactive \
    --executor "docker" \
    --docker-image alpine:lastest \
    --url "https://gitlab.mczaiyun.top" \
    --registration-token "vtizNrFzQKFacsSMxsJX" \
    --description "first-register-runner" \
    --tag-list "test-cicd, dockercicd" \
    --run-untagged="true" \
    --locked="false" \
    --access-level="not_protected"
```

`--rm -v /srv/gitlab-runner/config:/etc/gitlab-runner`
- 移除配置文件，重新注册

`gitlab/gitlab-runner`
- 官方提供的一个脚本

注册runner的三个重要参数
`--url "https://gitlab.mczaiyun.top" `
- 指向gitlab的地址

`--registration-token "vtizNrFzQKFacsSMxsJX"`
- 配置token 

`--tag-list "test-cicd, dockercicd"`
- 注册runner的一个标签，指定你的流水线在哪一个runner下运行，就必须要指定tags

3. 查看gitlab目前关联runner
![./pic/gitlab_runner_admin]
