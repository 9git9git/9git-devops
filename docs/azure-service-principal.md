## Azure 서비스 주체(Service Principal) 생성 가이드라인

> **Azure 서비스 주체(Service Principal)**는 애플리케이션, 서비스, 자동화 도구 등이 Azure 리소스에 안전하게 액세스하기 위해 사용하는 보안 ID입니다. 사용자 계정 대신 애플리케이션에 특정 역할과 권한을 부여하여 보안을 강화하고 자동화된 워크플로우를 구현하는 데 필수적입니다.

다음은 Azure 서비스 주체를 생성하는 주요 방법과 관련 가이드라인입니다.

### 1. 사전 준비 사항

* **활성화된 Azure 구독 (Azure Subscription):** 서비스 주체를 사용하고 리소스에 접근하려면 유효한 Azure 구독이 필요합니다.
* **권한:** Azure AD(현재 **Microsoft Entra ID**) 내에서 애플리케이션을 등록하고 서비스 주체를 생성하며, 역할을 할당할 수 있는 충분한 권한이 필요합니다. (예: 전역 관리자, 애플리케이션 관리자, 사용자 액세스 관리자 등)

### 2. 생성 방법

서비스 주체는 주로 다음 세 가지 방법을 통해 생성할 수 있습니다.

#### 방법 1: Azure Portal 사용

가장 직관적인 방법으로, GUI 환경에서 단계별로 생성할 수 있습니다.

1. **Azure Portal 로그인:** [portal.azure.com](https://portal.azure.com) 에 접속하여 로그인합니다.
2. **Microsoft Entra ID (Azure Active Directory) 이동:** 검색창에 "**Microsoft Entra ID**" (또는 "**Azure Active Directory**")를 검색하고 해당 서비스로 이동합니다.
3. **앱 등록 (App registrations) 선택:** 왼쪽 메뉴에서 '관리(Manage)' 섹션 아래의 '**앱 등록(App registrations)**'을 클릭합니다.
4. **새 등록 (+ New registration) 클릭:** 상단의 '**+ 새 등록**' 버튼을 클릭합니다.
5. **애플리케이션 등록:**
    - **이름 (Name):** 서비스 주체를 식별할 수 있는 이름을 입력합니다. (예: `my-automation-app-sp`)
    - **지원되는 계정 유형 (Supported account types):** 일반적으로 '**이 조직 디렉터리의 계정만(단일 테넌트)**' (Accounts in this organizational directory only)을 선택합니다. 특정 요구 사항에 따라 다른 옵션을 선택할 수 있습니다.
    - **리디렉션 URI (Redirect URI - Optional):** 웹 애플리케이션이나 특정 인증 흐름에 필요한 경우 플랫폼(웹, SPA 등)을 선택하고 URI를 입력합니다. 자동화 스크립트용이라면 보통 비워둡니다.
    - **등록 (Register):** '**등록**' 버튼을 클릭합니다.
6. **중요 정보 기록:** 앱 등록이 완료되면 '개요(Overview)' 페이지에 다음 정보가 표시됩니다. 이 값들을 안전한 곳에 기록해 두세요.
    - **애플리케이션(클라이언트) ID (Application (client) ID):** 서비스 주체의 고유 식별자입니다.
    - **디렉터리(테넌트) ID (Directory (tenant) ID):** 서비스 주체가 속한 Azure AD 테넌트의 ID입니다.
7. **클라이언트 암호 생성 (Client Secret):**
    - 왼쪽 메뉴에서 '**인증서 및 암호 (Certificates & secrets)**'를 클릭합니다.
    - '**클라이언트 암호 (Client secrets)**' 탭을 선택하고 '**+ 새 클라이언트 암호 (+ New client secret)**'를 클릭합니다.
    - **설명 (Description):** 암호의 용도를 알 수 있는 설명을 입력합니다. (예: `automation-secret`)
    - **만료 (Expires):** 암호의 만료 기간을 선택합니다. (보안을 위해 권장 기간 선택, 예: 6개월)
    - **추가 (Add):** '**추가**' 버튼을 클릭합니다.
    - **!!! 중요 !!!:** 클라이언트 암호가 생성되면 '**값(Value)**' 열에 암호 문자열이 표시됩니다. **이 값은 페이지를 벗어나면 다시 볼 수 없으므로, 즉시 복사하여 안전한 곳(예: Azure Key Vault, 보안된 비밀 관리 도구)에 저장해야 합니다.** '비밀 ID(Secret ID)'가 아니라 '**값(Value)**'을 복사해야 합니다.

#### 방법 2: Azure CLI 사용

> 명령줄 인터페이스를 선호하는 경우 **Azure CLI**를 사용하여 빠르고 자동화된 방식으로 생성할 수 있습니다.

1. **Azure CLI 설치 및 로그인:** Azure CLI가 설치되어 있지 않다면 설치하고, `az login` 명령어로 Azure 계정에 로그인합니다.
2. **서비스 주체 생성 명령어 실행:**
    - **역할 할당과 함께 생성 (권장):**   
        ```bash
        az ad sp create-for-rbac --name <ServicePrincipalName> --role <RoleName> --scopes /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>
        # 예시: 기여자(Contributor) 역할을 특정 리소스 그룹에 할당
        # az ad sp create-for-rbac --name my-automation-sp --role "Contributor" --scopes /subscriptions/your_subscription_id/resourceGroups/your_resource_group
        # 구독 전체에 역할을 할당하려면 --scopes /subscriptions/<SubscriptionID> 사용
        ```
    - **서비스 주체만 생성 (역할은 나중에 할당):**
        ```bash
        az ad sp create-for-rbac --name <ServicePrincipalName>
        ```
3. **출력 결과 기록:** 명령어 실행 후 JSON 형식으로 결과가 출력됩니다. 다음 값들을 기록해 두세요.
    - `appId`: **애플리케이션(클라이언트) ID**
    - `password`: **클라이언트 암호 (Client Secret)**
    - `tenant`: **디렉터리(테넌트) ID**

#### 방법 3: Azure PowerShell 사용

> PowerShell 스크립트를 사용하는 환경이라면 **Azure PowerShell** 모듈을 활용할 수 있습니다.

1. **Azure PowerShell 설치 및 로그인:** Azure PowerShell 모듈을 설치하고 `Connect-AzAccount` cmdlet으로 로그인합니다.
2. **서비스 주체 및 암호 생성 스크립트 실행:**
    ```powershell
    # 새 Azure AD 애플리케이션 생성
    $app = New-AzADApplication -DisplayName "<AppName>"

    # 해당 애플리케이션에 대한 서비스 주체 생성
    $sp = New-AzADServicePrincipal -ApplicationId $app.AppId

    # 서비스 주체에 대한 암호 생성 (Plain Text 반환)
    # 중요: $credential.SecretText 값을 안전하게 저장해야 합니다.
    $credential = New-AzADServicePrincipalPasswordCredential -ServicePrincipalId $sp.Id -DisplayName "MyPassword"
    $secretValue = $credential.SecretText

    # 필요한 정보 출력
    Write-Host "Application (Client) ID: $($app.AppId)"
    Write-Host "Tenant ID: $((Get-AzContext).Tenant.Id)" # 현재 컨텍스트의 테넌트 ID 확인
    Write-Host "Client Secret: $secretValue" # !!! 이 값을 즉시 안전하게 저장하세요 !!!

    # (선택 사항) 역할 할당 예시 (구독 범위에 Reader 역할 할당)
    # New-AzRoleAssignment -ObjectId $sp.Id -RoleDefinitionName "Reader" -Scope "/subscriptions/<SubscriptionID>"
    ```
3. **출력 및 암호 기록:** 스크립트 실행 결과로 출력되는 **`Application (Client) ID`**, **`Tenant ID`**, 그리고 변수 `$secretValue`에 저장된 **`Client Secret`** 값을 안전하게 기록합니다.

### 3. 역할 할당 (Role Assignment)

> 서비스 주체를 생성하는 것만으로는 Azure 리소스에 접근할 권한이 없습니다. 서비스 주체가 특정 작업을 수행할 수 있도록 **Azure 역할 기반 접근 제어(RBAC)**를 사용하여 적절한 역할(예: 읽기 권한자(Reader), 기여자(Contributor), 소유자(Owner) 또는 사용자 지정 역할)을 필요한 범위(구독, 리소스 그룹, 특정 리소스)에 할당해야 합니다.

- **Azure Portal:** 대상 범위(예: 구독, 리소스 그룹)로 이동 -> '**Access control (IAM)**' 메뉴 -> '**+ 추가 (+ Add)**' -> '**역할 할당 추가 (Add role assignment)**' -> 역할 선택 -> '**멤버**' 탭에서 '**서비스 주체**' 선택 및 생성한 서비스 주체 이름 검색/선택 -> 검토 + 할당.
- **Azure CLI:** `az role assignment create --assignee <AppId> --role <RoleName> --scope <Scope>` 명령어를 사용합니다. (`az ad sp create-for-rbac` 사용 시 `--role` 및 `--scopes` 옵션으로 이미 할당했을 수 있습니다.)
- **Azure PowerShell:** `New-AzRoleAssignment -ObjectId <ServicePrincipalObjectId> -RoleDefinitionName <RoleName> -Scope <Scope>` cmdlet을 사용합니다. (위 PowerShell 예제에 포함되어 있습니다.)

### 4. 보안 및 관리 고려 사항

- **최소 권한 원칙:** 서비스 주체에는 필요한 작업을 수행하는 데 필요한 **최소한의 권한만** 부여해야 합니다.
- **클라이언트 암호 관리:**
  - 생성된 **클라이언트 암호**는 매우 중요하므로 Azure Key Vault와 같은 보안 저장소에 안전하게 보관하세요.
  - 암호에 만료 기간을 설정하고, 만료되기 전에 정기적으로 **교체(Rotation)**하는 프로세스를 마련하세요.
  - 가능하다면 암호 대신 **인증서 기반 인증**을 사용하는 것이 더 안전할 수 있습니다.
- **범위 제한:** 가능한 가장 **좁은 범위**(예: 특정 리소스 그룹 또는 리소스)에 역할을 할당하세요. 구독 전체에 광범위한 권한을 부여하는 것은 피하는 것이 좋습니다.
- **모니터링 및 감사:** Azure Monitor 및 Microsoft Entra ID(Azure AD) 로그인 로그를 통해 서비스 주체의 활동을 정기적으로 모니터링하고 감사합니다.
- **정리:** 더 이상 사용되지 않는 서비스 주체는 보안 위험을 줄이기 위해 삭제하세요.

이제 이 가이드라인을 따라 Azure 환경에서 애플리케이션과 자동화 작업을 위한 서비스 주체를 효과적으로 생성하고 관리할 수 있습니다.