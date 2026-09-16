
# Jupyter





3. 使用远程 JupyterServer

&emsp;&emsp;首先在虚拟机中输入 `jupyter notebook --generate-config` 命令，创建 jupyter 配置文件，该文件会创建在用户家目录 `/.jupyter` 目录下：

![16](./images/00/28.png)

&emsp;&emsp;接下来创建登录密码，在命令行中输入如下命令，之后输入两次密码，密码会写入配置文件中：

```shell
jupyter-lab password
```

![](./images/00/29.png)

&emsp;&emsp;接下来修改上面生成的配置文件，添加配置。进入到配置文件：

```shell
vim /home/atguigu/.jupyter/jupyter_notebook_config.py
```

&emsp;&emsp;接下来添加如下配置：

```text
c.ServerApp.ip = '*'  # 允许所有ip访问
c.ServerApp.open_browser = False  # 不自动打开浏览器
c.ServerApp.port = 8888  # 指定端口,默认8888
```

![](./images/00/30.png)

&emsp;&emsp;保存并退出后，在虚拟机中输入 `jupyter lab` 命令，启动 jupyter server，启动后会返回访问链接，远程访问时需要将链接中的 localhost 或 127.0.0.1 修改为虚拟机的主机名或虚拟机 ip：

![](./images/00/31.png)

&emsp;&emsp;然后在浏览器中输入链接并输入密码，远程使用 jupyter notebook：

![](./images/00/32.png)

&emsp;&emsp;PyCharm 中打开【Settings】设置，在【Languages & Frameworks】下的【Jupyter】下的【Jupyter Servers】中的【Configured Server】中填写链接并输入密码：

![](./images/00/33.png)

![](./images/00/34.png)

&emsp;&emsp;在 PyCharm 中创建一个 `.ipynb` 文件：

![](./images/00/35.png)

&emsp;&emsp;也可以在 `.ipynb` 文件上方配置远程 Jupyter 服务链接：

![](./images/00/36.png)

4. Jupyter 快捷键

+ esc：从输入模式退出到命令模式
+ a：在当前cell上面创建一个新的cell
+ b：在当前cell 下面创建一个新的cell
+ dd：删除当前cell
+ m：切换到markdown模式
+ y：切换到code模式
+ ctrl+回车：运行cell 
+ shift +回车：运行当前cell并创建一个新的cell