# Tutorial

envd is a container-based development environment management tool for data scientists.

🐍 **No docker, only python** - Write python code to build the development environment, we help you take care of Docker.

🖨️ **Built-in jupyter/vscode** - Jupyter and VSCode remote extension are the first-class support.

⏱️ **Save time** - Better cache management to save your time, keep the focus on the model, instead of dependencies

☁️ **Local & cloud** - Run the environment locally or in the cloud, without any code change

🐳 **Container native** - Leverage container technologies but no need to learn how to use them, we optimize it for you

🤟 **Infrastructure as code** - Describe your project in a declarative way, 100% reproducible

Let's discover **envd in less than 5 minutes**.

## Getting Started

Get started by **creating a new envd environment**.

### What you'll need

- Docker (20.10.0 or above)

### Install envd

You can download the binary from the [latest release page](https://github.com/tensorchord/envd/releases/latest), and add it in `$PATH`.

After the download, please run `envd bootstrap` to bootstrap.

### Create an envd environment

Please clone the [`envd-quick-start`](https://github.com/tensorchord/envd-quick-start):

```
git clone https://github.com/tensorchord/envd-quick-start.git
```

The build manifest `build.envd` looks like:

```python
def build():
    base(dev=True)
    install.conda()
    install.python()
    install.python_packages(name = [
        "numpy",
    ])
    shell("fish")
```

Then please run the command below to setup a new environment:

```
cd envd-quick-start && envd up
```

```bash
$ cd envd-quick-start && envd up
[+] ⌚ parse build.envd and download/cache dependencies 6.2s ✅ (finished) 
[+] build envd environment 19.0s (47/47) FINISHED                                                 
 => CACHED [internal] setting pip cache mount permissions                                     0.0s
 => docker-image://docker.io/tensorchord/envd-sshd-from-scratch:v0.4.3                        2.3s
 => => resolve docker.io/tensorchord/envd-sshd-from-scratch:v0.4.3                            2.3s
 => docker-image://docker.io/library/ubuntu:22.04                                             0.0s
......
 => [internal] pip install numpy                                                              2.5s
 => CACHED [internal] download fish shell                                                     0.0s
 => [internal] configure user permissions for /opt/conda                                      1.0s
 => [internal] create dir for ssh key                                                         0.5s
 => [internal] install ssh keys                                                               0.2s
 => [internal] copy fish shell from the builder image                                         0.2s
 => [internal] install fish shell                                                             0.5s
......
 => [internal] create work dir: /home/envd/envd-quick-start                                   0.2s
 => exporting to image                                                                        7.7s
 => => exporting layers                                                                       7.7s
 => => writing image sha256:464a0c12759d3d1732404f217d5c6e06d0ee4890cccd66391a608daf2bd314e4  0.0s
 => => naming to docker.io/library/envd-quick-start:dev                                       0.0s
------
 > importing cache manifest from docker.io/tensorchord/python-cache:envd-v0.4.3:
------
⣽ [5/5] attach the environment  [2s]            
Welcome to fish, the friendly interactive shell
Type help for instructions on how to use fish

envd-quick-start on git master [!] via Py v3.11.11 via 🅒 envd as sudo 
⬢ [envd]❯ # You are in the container-based environment!
```

### Play with the environment

You can run `ssh envd-quick-start.envd` to reconnect if you exit from the environment. Or you can execute `git` or `python` commands inside.

```bash
$ python demo.py
[2 3 4]
$ git fetch
$
```

### Setup jupyter notebook

Please edit the `build.envd` to enable jupyter notebook:

```python
def build():
    base(dev=True)
    install.conda()
    install.python()
    install.python_packages(name = [
        "numpy",
    ])
    shell("fish")
    config.jupyter(password="", port=8888)
```

You can get the endpoint of jupyter notebook via `envd get envs`.

```bash
$ envd up --detach
$ envd get env
NAME                    JUPYTER                 SSH TARGET              CONTEXT                                 IMAGE                   GPU     CUDA    CUDNN   STATUS          CONTAINER ID 
envd-quick-start        http://localhost:8888   envd-quick-start.envd   /home/gaocegege/code/envd-quick-start   envd-quick-start:dev    false   <none>  <none>  Up 54 seconds   bd3f6a729e94
```
