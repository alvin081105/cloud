aws홈페이지에서 vpc검색 → vpc생성 → 생성할 리소스(VPC만), CIDR블럭 10.0.0.0/16으로 설정 → vpc생성 

- 서버는 서울서버 타 서버 선택 주의!!!

서브넷 생성 →
![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/1d9aba2b-7aef-459c-9952-47ce9796e86b/2f07991a-bc72-42c0-965b-b29e6d985b43/image.png)
VPC ID에서 방금만든 vpc선택 → 아래처럼 서브넷 설정(총 4개)

public subnet a

- 이름 : public-subnet-a
- 가용 영역 : ap-northeast-2a
- CIDR : 10.0.0.0/24

public subnet b

- 이름 : public-subnet-b
- 가용 영역 : ap-northeast-2b
- CIDR : 10.0.1.0/24

private subnet a

- 이름 : private-subnet-a
- 가용 영역 : ap-northeast-2a
- CIDR : 10.0.2.0/24

private subnet b

- 이름 : private-subnet-b
- 가용 영역 : ap-northeast-2b
- CIDR : 10.0.3.0/24

vpc가 인터넷과 통신할 수 있도록 internet gateway를 구성

인터넷 게이트웨이 생성 → 이름태그를 igw로 설정 → 인터넷 게이트웨이 생성  

방금 만든 인터넷게이트웨이 선택후 작업에서 vpc연결 클릭후 자신이 만든 vpc선택후 인터넷게이트웨이 생성하기

이렇게 되면 vpc와 인터넷게이트웨이가 통신할 기반이 마련됨

public subnet에서도 필요한 패키지등을 다운받기위해서는 인터넷이 필요하므로 NAT gateway를 생성

NAT gateway는 private subnet이 인터넷게이트웨이와 통신을 할수있도록 만들어주는 징검다리와 같은 역할을 합니다.

NAT 게이트웨이 생성 → NAT 게이트웨이의 이름을 nat-a로 설정 → 서브넷을 연결하고자하는 private subnet을 선택 → 탄력적 IP할당(실수나 고의로 여러번 눌러서 여러개 생성시 사용을 안해도 돈이 나가니 탄력적ip에서 잘못할당받은 ip들을 선택후 작업에서 릴리즈를 선택하여 지울것!!!) → 이미 있을시 그것을 선택후 NAT 게이트웨이 생성

이젠 인터넷 통신을 위한 라우팅 설정을 한다

라우팅 테이블 생성 → 이름을 public-rt로 설정, vpc는 만들어놓은 vpc선택 → 아래와 같이 테이블 구성

- public-rt
- private-rt-a
- private-rt-b
-완성본

다음으로 라우팅과 서브넷을 연결을 해줍니다.

public-rt를 선택후 아래의 라우팅 → 라우팅 편집 → 라우팅 추가(대상 : 0.0.0.0/0, 대상 : 인터넷 게이트웨이 - 아까 만들어두었던 인터넷 게이트웨이 선택) → 변경사항 저장
나머지 라우팅 테이블들은 private서버에 연결을 위해 대상을 인터넷 게이트웨이가 아닌 아까 만들어두었던 NAT 게이트웨이로 설정을 합니다.
이제 public-rt를 선택후 서브넷 연결 → 서브넷 연결 편집

각각의 라우팅 테이블에 맞는 서브넷을 연결합니다.

- public-rt ← public-subnet-a, public-subnet-b
- private-rt-a ← private-subnet-a
- private-rt-b ← private-subnet-b

이렇게 완성했으면 기본 네트워크 설정은 끝났습니다.




