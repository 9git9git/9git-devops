# Azure Database for PostgreSQL

> `Azure Database for PostgreSQL`는 클라우드에서 관계형 데이터베이스를 신속하게 만들고 관리할 수 있는 관리형 데이터베이스 서비스입니다.

## Azure Database for PostgreSQL 선택 이유

Azure Database for PostgreSQL을 선택하는 이유는 다음과 같습니다:

- **관리형 서비스**: 인프라 관리, 패치, 백업 및 복구를 자동으로 처리하여 개발자가 데이터베이스 관리에 대한 부담을 덜 수 있습니다.
- **확장성**: 필요에 따라 리소스를 쉽게 확장하거나 축소할 수 있어 비즈니스 성장에 유연하게 대응할 수 있습니다.
- **고가용성**: 여러 지역에 데이터베이스를 배포할 수 있어 장애 발생 시에도 서비스의 가용성을 유지합니다.
- **보안**: 데이터 암호화, 네트워크 보안, 사용자 인증 및 권한 관리와 같은 다양한 보안 기능을 제공합니다.
- **통합 및 호환성**: Azure의 다른 서비스와 쉽게 통합할 수 있으며, PostgreSQL의 표준 기능을 지원합니다.

## 사용 사례

- **웹 애플리케이션**: 사용자 데이터, 세션 정보 및 로그 데이터를 저장하는 데 적합합니다.
- **데이터 분석**: 대량의 데이터를 처리하고 분석하는 데 필요한 성능과 확장성을 제공합니다.
- **모바일 애플리케이션**: 사용자 데이터를 안전하게 관리하고 동기화할 수 있습니다.
- **IoT 애플리케이션**: 실시간 데이터 분석 및 모니터링을 지원합니다.

# Azure Database for PostgreSQL 리소스 생성

- Azure Portal에서 `Azure Database for PostgreSQL` 리소스를 생성할 수 있습니다.

![Create Azure Database for PostgreSQL](./images/azure-database-for-postgresql/01.png){: .align-center alt="Azure Portal showing the option to create a new PostgreSQL database resource"}

- 리소스 만들기를 클릭하면 리소스 생성 페이지로 이동합니다.
  - 리소스 그룹, 서버 이름, 고가용성, 인증 방법, 관리자 로그인, 암호, 암호 확인을 입력합니다.
  - 이미 환경에 익숙하신 분들은 원하는 환경에 맞게 설정합니다.
- 설정을 다하면 `다음` 버튼을 클릭합니다.

![Create Azure Database for PostgreSQL](./images/azure-database-for-postgresql/02.png){: .align-center alt="Configuration options for the PostgreSQL database resource"}
![Create Azure Database for PostgreSQL](./images/azure-database-for-postgresql/03.png){: .align-center alt="Networking configuration options for the PostgreSQL database resource"}

- 연결 방법은 이후에 바꿀 수 없으므로 `공용 액세스(허용된 IP 주소) 및 프라이빗 엔드포인트` 방법을 선택합니다.
  - 현재 클라이언트 컴퓨터의 IP 주소를 허용하는 것으로 설정합니다.
  - 이후에 엑세스 가능한 IP 주소를 추가할 수 있습니다.
- `검토 + 만들기` 버튼을 클릭합니다.

![Create Azure Database for PostgreSQL](./images/azure-database-for-postgresql/04.png){: .align-center alt="Review and create screen for the PostgreSQL database resource"}

- 배포는 몇 분 정도 걸립니다.

# Azure Database for PostgreSQL 기본 데이터베이스 생성

- 배포가 완료되면 `Azure Database for PostgreSQL` 리소스로 이동합니다.
- 설정 탭에서 `데이터베이스` 탭을 클릭합니다.

![Create Azure Database for PostgreSQL](./images/azure-database-for-postgresql/05.png){: .align-center alt="Database tab in the PostgreSQL resource settings"}

- `추가` 버튼을 클릭합니다.
- 데이터베이스 이름을 입력하고 `저장` 버튼을 클릭합니다.

![Create Azure Database for PostgreSQL](./images/azure-database-for-postgresql/06.png){: .align-center alt="Adding a new database to the PostgreSQL resource"}

# 참고 자료

- [Azure Database for PostgreSQL](https://learn.microsoft.com/en-us/azure/postgresql/)