# Table of Contents

- [Xây dựng tài nguyên lên cụm để chạy một ứng dụng đơn giản](#xây-dựng-tài-nguyên-lên-cụm-để-chạy-một-ứng-dụng-đơn-giản)
  - [Đóng gói ứng dụng và đưa lên Docker Hub](#đóng-gói-ứng-dụng-và-đưa-lên-docker-hub)
    - [1. Đóng gói container](#1-đóng-gói-container)
      - [Khởi động Docker desktop và Sử dụng lệnh docker build để tạo image](#khởi-động-docker-desktop-và-sử-dụng-lệnh-docker-build-để-tạo-image)
      - [Chạy container bằng lệnh docker run](#chạy-container-bằng-lệnh-docker-run)
      - [Sử dụng lệnh curl](#sử-dụng-lệnh-curl)
    - [2. Chia sẻ ứng dụng lên dockerhub](#2-chia-sẻ-ứng-dụng-lên-dockerhub)
      - [Gắn tag cho ứng dụng bằng lệnh docker tag](#gắn-tag-cho-ứng-dụng-bằng-lệnh-docker-tag)
      - [Sử dụng lệnh docker push để đưa ứng dụng lên docker hub](#sử-dụng-lệnh-docker-push-để-đưa-ứng-dụng-lên-docker-hub)
  - [Xây dựng tài nguyên lên cụm Kubernetes](#xây-dựng-tài-nguyên-lên-cụm-kubernetes)
    - [1. Tạo file first-app.yaml và tạo Deployment và Service bằng câu lệnh](#1-tạo-file-first-appyaml-và-tạo-deployment-và-service-bằng-câu-lệnh)
    - [2. Kiểm tra trạng thái các tài nguyên vừa tạo](#2-kiểm-tra-trạng-thái-các-tài-nguyên-vừa-tạo)
      - [Kiểm tra hoạt động của các Pod bằng câu lệnh](#kiểm-tra-hoạt-động-của-các-pod-bằng-câu-lệnh)
      - [Kiểm tra hoạt động của các Service bằng câu lệnh](#kiểm-tra-hoạt-động-của-các-service-bằng-câu-lệnh)
      - [Kiểm tra bằng K9s](#kiểm-tra-bằng-k9s)
    - [3. Mở ứng dụng vừa tạo](#3-mở-ứng-dụng-vừa-tạo)
      - [Sử dụng câu lệnh theo Service loại NodePort](#sử-dụng-câu-lệnh-theo-service-loại-nodeport)
      - [Thực hiện mở ứng dụng bằng lệnh](#thực-hiện-mở-ứng-dụng-bằng-lệnh)
      - [Có thể thực hiện lệnh curl](#có-thể-thực-hiện-lệnh-curl)



# Xây dựng tài nguyên lên cụm để chạy một ứng dụng đơn giản

## Đóng gói ứng dụng và đưa lên `Docker Hub`
### 1. Đóng gói container
#### Khởi động Docker desktop và Sử dụng lệnh `docker build` để tạo image: 
```
docker build -t first-app .

```
![This is an alt text.](https://i.imgur.com/nJVN94Z.png "This is a sample image.")
#### Chạy container bằng lệnh `docker run`:

```
docker run -d -p 127.0.0.1:80:80 first-app

```

Ta kiểm tra trên docker desktop thấy container ở trạng thái `Running` có nghĩa là đã tạo `image` thành công :

![This is an alt text.](https://i.imgur.com/rll5cAZ.png "This is a sample image.")

sử dụng lệnh `curl`:
```
curl http://127.0.0.1:80
```
![This is an alt text.](https://i.imgur.com/qdf2KvT.png "This is a sample image.")
### 2. Chia sẻ ứng dụng lên dockerhub
#### Gắn `tag` cho ứng dụng bằng lệnh `docker tag`: 
```
docker tag first-app thanhlongnguyen1012/first-app  
```
#### Sử dụng lệnh `doker push` để đưa ứng dụng lên `docker hub':
```
docker push thanhlongnguyen1012/first-app
```
![This is an alt text.](https://i.imgur.com/sOSR7Gk.png "This is a sample image.")
#### Mở `docker hub` kiểm tra image đã được chia sẻ thành công: 
![This is an alt text.](https://i.imgur.com/ULjg5vp.png "This is a sample image.")
## Xây dựng tài nguyên lên cụm Kubernetes
### 1. Tạo file `first-app.yaml` và tạo Deployment và Service bằng câu lệnh:
```
kubectl apply -f first-app.yaml
```
### 2. Kiểm tra trạng thái các tài nguyên vừa tạo: 
#### Kiểm tra hoạt động của các `Pod` bằng câu lệnh: 
```
kubectl get pod
```
#### Kiểm tra hoạt động của các `Service` bằng câu lệnh: 
```
kubectl get service
```
![This is an alt text.](https://i.imgur.com/l7EuYsF.png "This is a sample image.")
#### Kiểm tra bằng K9s
Ta có thể dùng công cụ trực quan `k9s` để quản lý cụm thay cho công cụ dòng lệnh `kubectl`.

Khởi động `k9s bằng lệnh:

```
minikube k9s

```
![This is an alt text.](https://i.imgur.com/iwumQly.png "This is a sample image.")
### 3. Mở ứng dụng vừa tạo
Với cụm có `<Ip-Node>`có thể sử dụng câu lệnh theo `Service` loại `NodePort`: 
```
http://<nodeIP>:nodePort
```
Kiểm tra `<Ip-Node>` hay` EXTERNAL-IP`sau khi thực hiện câu lệnh: 
```
kubectl get nodes -o wide 
```
![This is an alt text.](https://i.imgur.com/y7wt3nI.png "This is a sample image.")

Trong cụm `minikube` không có ` EXTERNAL-IP` nên ta thực hiện mở ứng dụng bằng lệnh: 
```
minikube service static-web1-service

```
![This is an alt text.](https://i.imgur.com/frc7eWl.png "This is a sample image.")
Câu lệnh trên mở browser mặc định:

![This is an alt text.](https://i.imgur.com/kAzuCkS.png "This is a sample image.")
Có thể thực hiện lệnh `curl`: 
```
http://127.0.0.1:63158
```
![This is an alt text.](https://i.imgur.com/J96cYK0.png "This is a sample image.")

