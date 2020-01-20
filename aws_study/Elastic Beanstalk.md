## 3. Elastic Beanstalk 사용 시작(Platform은 도커로)

### ***1. 애플리케이션 생성***

>- 다음 링크를 사용하여 Elastic Beanstalk 콘솔을 엽니다: https://console.aws.amazon.com/elasticbeanstalk/home#/gettingStarted?applicationName=getting-started-app
>- 이름이 **getting-started-app**인 Elastic Beanstalk 애플리케이션을 생성
>- 플랫폼은 도커로
>
>- AWS 리소스가 있으며 이름이 **GettingStartedApp-env**인 환경을 시작
>
>

### 2. 새버전의 애플리케이션 배포

>- 환경 플랫폼과 일치하는 샘플 애플리케이션을 다운로드
>- Elastic Beanstalk 애플리케이션 페이지에서 **getting-started-app**을 선택한 다음 **GettingStartedApp-env**를 선택합니다.
>- **개요** 섹션에서 **업로드 및 배포**를 선택
>- **파일 선택**을 선택한 다음 다운로드한 샘플 애플리케이션 소스 번들을 업로드
>- 배포 선택

![3](https://user-images.githubusercontent.com/37895303/72699050-3d686980-3b8a-11ea-9fad-88bb668831cf.PNG)

### 3. 환경 구성

>이 구성 변경 예에서는 환경의 용량 설정을 편집합니다. Auto Scaling 그룹에 2 ~ 4개의 Amazon EC2 인스턴스가 있고 로드 밸런싱 수행 및 자동 조정 환경을 구성한 다음 변경이 발생했는지 확인합니다. Elastic Beanstalk는 추가 Amazon EC2 인스턴스를 생성하여 처음 생성한 단일 인스턴스에 추가합니다. 그런 다음 Elastic Beanstalk는 두 인스턴스를 환경의 로드 밸런서와 연결합니다. 결과적으로 애플리케이션의 응답성이 향상되고 가용성이 향상됩니다.
>
>**환경 용량을 변경하려면 다음을 수행합니다.**
>
>1. [Elastic Beanstalk 콘솔](https://console.aws.amazon.com/elasticbeanstalk)을 엽니다.
>
>2. 해당 환경의 [관리 페이지](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/environments-console.html)로 이동합니다.
>
>3. [**Configuration**]을 선택합니다.
>
>4. **용량** 구성 범주에서 **수정**을 선택합니다.
>
>5. **Auto Scaling 그룹** 섹션에서 **환경 유형**을 **로드 밸런싱 수행**으로 변경합니다.
>
>6. **인스턴스** 행에서 **최대**를 `4`로 변경하고 **최소**를 `2`로 변경합니다.
>
>7. **Modify capacity(용량 수정)** 페이지에서 **저장**을 선택합니다.
>
>8. **Configuration overview(구성 개요)** 페이지에서 **적용**을 선택합니다.
>
>9. 이 업데이트가 현재 인스턴스를 모두 대체한다는 경고가 표시됩니다. [**Confirm**]을 선택합니다.
>
>10. 탐색 창에서 **이벤트**를 선택합니다.
>
>    환경이 업데이트되는 데 몇 분이 걸릴 수 있습니다. 완료되었는지 알아보려면 이벤트 목록에서 이벤트 **Successfully deployed new configuration to environment(새 구성이 환경에 성공적으로 배포되었습니다)**를 찾아봅니다. 이를 통해 Auto Scaling 최소 인스턴스 개수가 2로 설정되었음을 확인합니다. Elastic Beanstalk는 자동으로 두 번째 인스턴스를 시작합니다.

![4](https://user-images.githubusercontent.com/37895303/72699626-84575e80-3b8c-11ea-8a33-c2f3f61968a6.PNG)



## 

