


## 





## 
- BOSH가 VM을 생성할 때 YML파일을 참조하는데 그 YML파일은 다양한 `configs`를 참조하여 완성됨
- 사용자는 customizing을 위해 이 configs를 수정할 수 있는데, 그 과정은 아래와 같음

- 




* configs 관리
```
bosh configs    #configs 내역을 조회할 수 있음
bosh delete-config [번호 or --name=]   #삭제하기
bosh update-config [내가 생성한 yml파일] --name=[생성하고 싶은이름] --type=[runtime or cpi or cloud]

```

`bosh update-runtime-config`와 `bosh update-config --type=[runtime]` 동일한 의미

* 기존 configs 수정하기
```
bosh config [수정할 대상 번호] > [파일명]
```

bosh는 stemcell을 업로드 함



* manifest 파일 수정하기
- 기존에 제공된 manifest.yml 파일에 configs에 있는 정보들을 넣어준다.
- vm type, stemcell, NW명 등을 넣어줘야 정상적으로 동작한다.
- 이렇게 yml파일에 값을 넣어주면, manifest.yml -> configs의 정보를 읽어와 배포한다.

```

```


* bosh deploy
```
bosh -d [deploy이름] deploy [yml파일위치] --vars-store=[변수 저장값] -v [변수명]=[변수값]
```
Question
** kes runtime config 변경된 것은 어떻게 하면 알수 있는지 
** deploy 했을 때 변경되는 정보들이 나오는데 비교를 할 수 있는지
** vm type이나 기타 정보들을 runtime conf에서 굳이 다 가져와서 해야하는지?



