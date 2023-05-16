# Jupyter远程访问

## 使用ssh远程使用
* 在服务器上运行`jupyter notebook -no-browser --port=8080` 
* 在本地终端运行`ssh -NfL localhost:port:localhost:port -p sshport username@ip`
> 第一个是本地，第二个是远程
> 将本地的localhost:port映射到远程的localhost:port上
> N代表没有命令要被远程执行，f代表后台执行，L是端口映射操作
* 打开浏览器访问`https://localhost:port/`

## 使用官方工具
* 生成默认配置文件`jupyter notebook --generate-config`
* 生成访问密码，首先进入`ipython`
```python
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:xxxxxxxxxxxxxxxxx'
```
* 修改`./jupyter/jupyter_notebook_config.py`修改如下
```
c.NotebookApp.ip='*'
c.NotebookApp.password = u'sha:ce...刚才复制的那个密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888 #可自行指定一个端口, 访问时使用该端口
```
> `notebook 5.0`以后自带命令完成2 3步
> ```
> $ jupyter notebook password
> Enter password:  ****
> Verify password: ****
> [NotebookPasswordApp] Wrote hashed password to /Users/you/.jupyter/jupyter_notebook_config.json
> ```

* 访问`https://ip:port/`

