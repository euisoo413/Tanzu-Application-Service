## buildpack


## cf buildpack 위치
- nfs_server (blobstore)  
  - ssh nfs_server
  - cd /var/vcap/store : 영구
  - cd /var/vcap/data : 휘발성데이터 

## buildpack customizing

- Buildpack 구조를 만들어서 빌드를 한다. (빌드라 함은 zip파일 만드는 느낌)
```
bundle install
bundle exec rake clean package OFFLINE=TRUE

```

```
cf create-buildpack ["빌드팩명'] ['번호'] [압축파일 위치, 빌드된 위치] 
```

## 빌드팩의 적용 
- `cf push` 과정에서 어떤 동작이 이루어지는지 살펴본다.
```
pushing app org ~~ space ~~
applying manifest file
packaging files to upload
uploading files..
======================> 100%

Staging app and tracing logs,..
  Download "Buildpack"
  cell "ASdniuasdnaf [diego-cell] creating container for instance asdasdi-asdasdasd-123ndu12=
  cell "ASdniuasdnaf [diego-cell] successfully container for instance asdasdi-asdasdasd-123ndu12= 
  Downloading/ed app package..
  Downloading/ed build artifacts cache..
  ----->Buildpack version 4.4.44
  -----------#빌드팩 내부에서 도는 스크립트---------------
  HTTPD ~~
  Downloaed 등의 과정을 거치는데 이 때 로컬을 참조하기도하고, 
  -------------------------------------------------
  uploading droplets, build artifacts cache..
 
  app starting 
  --------------------------------
```




