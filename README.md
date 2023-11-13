生成bfb文件

首先克隆Mellanox的创建bfb的库：

```
git clone https://github.com/Mellanox/bfb-build
cd bfb-build
```

对于生产与开发，有两种不同的创建模式：（distro是系统发行版，version是对应版本）

```
IMAGE_TYPE=prod ./bfb-build <distro> <version>
IMAGE_TYPE=dev ./bfb-build <distro> <version>
```

BFB 在 /tmp/<distro>/<version>.<pid> 目录下创建，pid在bfb创建时获得。

在上述执行过程中，会运行一个Dockfile来创建特定镜像，如果测试环境无法连接外网，需要对Dockerfile进行修改：

原本代码：

```
RUN wget -qO - https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/kubernetes-archive-keyring.gpg add -
RUN echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list
```

修改后代码：

```
RUN wget -qO - https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/kubernetes-archive-keyring.gpg add -
RUN echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list
```

执行完便可获得bfb文件。
