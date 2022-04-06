# VSCodeでDockerを用いたJupyter環境の構築
以下の```Dockerfile```を使ってイメージを作る  
```disable_check_xsrf```を```True```にしないとVSCodeでエラーが出る
```docker
RUN apt-get update
RUN apt-get install -y python3 python3-pip
RUN pip3 install jupyter
RUN jupyter notebook --generate-config
RUN sed -ie 's/# c.NotebookApp.disable_check_xsrf = False/c.NotebookApp.disable_check_xsrf = True/' ~/.jupyter/jupyter_notebook_config.py
```
```
docker build -t jupyter .
```
以下の```docker-compose.yml```を使ってjupyter notebookを立ち上げる
```
version: "3"

services:
  jupyter:
    image: jupyter:latest
    runtime: nvidia
    ports:
      - 8080:8888
    entrypoint: jupyter notebook --allow-root --ip=0.0.0.0 --port=8888 --no-browser --NotebookApp.token=''
```
VSCodeでCtrl+Shift+Pを押して```Jupyter: Specify Jupyter server for connections```を選択  
```Existing```を選択して以下を入力
```
http://localhost:8080/?token=null
```
VSCodeからDocker環境内のJupyterに接続できる
