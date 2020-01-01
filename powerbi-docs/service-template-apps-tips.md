---
title: Power BI에서 템플릿 앱 작성 팁
description: 좋은 템플릿 앱을 만들기 위한 쿼리, 데이터 모델, 보고서 및 대시보드 작성에 대한 팁
author: teddybercovitz
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-service
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: tebercov
ms.openlocfilehash: 04b50882c28bf561e628e9f02dff6c147233d260
ms.sourcegitcommit: 08b73af260ded51daaa6749338cb85db2eab587f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74099757"
---
# <a name="tips-for-authoring-template-apps-in-power-bi"></a>Power BI에서 템플릿 앱 작성 팁

Power BI에서 [템플릿 앱을 작성](service-template-apps-create.md)할 때, 그 중 일부는 작업 영역 생성, 테스트 및 프로덕션의 계획입니다. 하지만 다른 중요한 부분은 분명히 보고서와 대시보드를 작성하는 것입니다. 작성 프로세스를 네 가지 주요 구성 요소로 나눌 수 있습니다. 이러한 구성 요소로 작업하면 최상의 템플릿 앱을 만들 수 있습니다.

* **쿼리**를 사용하면 데이터를 [연결](desktop-connect-to-data.md) 및 [변환](desktop-query-overview.md)하고 [매개 변수](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/)를 정의할 수 있습니다. 
* **데이터 모델**에서 [관계](desktop-create-and-manage-relationships.md), [측정값](desktop-measures.md) 및 Q&A 개선 사항을 생성합니다.  
* **[보고서 페이지](desktop-report-view.md)** 에는 데이터에 대한 인사이트를 제공하는 시각적 개체 및 필터가 포함되어 있습니다.  
* **[대시보드](consumer/end-user-dashboards.md)** 및 [타일](service-dashboard-create.md)은 포함된 인사이트에 대한 개요를 제공합니다.
* 샘플 데이터를 사용하면 설치 후 즉시 앱을 검색할 수 있습니다.

기존 Power BI 기능으로 각 구성 요소에 익숙할 수 있습니다. 템플릿 앱을 빌드할 때 각 요소에 대해 고려해야 할 추가 사항이 있습니다. 자세한 내용은 아래의 각 섹션을 참조하세요.

<a name="queries"></a>

## <a name="queries"></a>쿼리
템플릿 앱의 경우 Power BI Desktop에서 개발된 쿼리를 사용하여 데이터 원본에 연결하고 데이터를 가져옵니다. 이러한 쿼리는 일관된 스키마를 반환하는 데 필요하며 예약된 데이터 새로 고침에 대해 지원됩니다(DirectQuery는 지원되지 않음).

### <a name="connect-to-your-api"></a>API에 연결
시작하려면 Power BI Desktop에서 API에 연결하여 쿼리 작성을 시작해야 합니다.

API에 연결하려면 Power BI Desktop에서 사용할 수 있는 데이터 커넥터를 사용할 수 있습니다. 웹 데이터 커넥터(데이터 가져오기 -> 웹)를 사용하여 OData 피드에 연결하도록 Rest API 또는 OData 커넥터(데이터 가져오기 -> OData 피드)에 연결할 수 있습니다.

> [!NOTE]
> 현재 템플릿 앱은 사용자 지정 커넥터를 지원하지 않습니다. 일부 연결 사용 사례의 경우 완화책으로 Odatafeed Auth 2.0을 사용하여 탐색하거나 인증을 위해 커넥터를 제출하는 것이 좋습니다. 커넥터를 개발하고 인증하는 방법에 대한 자세한 내용은 [데이터 커넥터 설명서](https://aka.ms/DataConnectors)를 참조하세요.

### <a name="consider-the-source"></a>소스 고려
쿼리는 데이터 모델에 포함된 데이터를 정의합니다. 시스템의 크기에 따라 이러한 쿼리는 고객이 비즈니스 시나리오에 따라 관리하기 쉬운 크기를 처리하도록 해주는 필터도 포함합니다.

Power BI 템플릿 앱에서는 여러 쿼리를 병렬로, 여러 사용자에 대해 동시에 실행할 수 있습니다.  제한 및 동시성 전략을 미리 계획하고 템플릿 앱을 내결함성 있게 만드는 방법을 문의하세요.

### <a name="schema-enforcement"></a>스키마 적용
쿼리는 시스템 변경에 대해 복원력이 있어야 하며 새로 고침 시 스키마 변경으로 모델이 손상될 수 있습니다. 소스에서 일부 쿼리에 대해 Null 또는 누락된 스키마 결과를 반환할 수 있는 경우 빈 테이블 또는 의미 있는 사용자 지정 오류 메시지를 반환하는 것을 고려합니다.

### <a name="parameters"></a>매개 변수
Power BI Desktop에서 [매개 변수](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/)를 통해 사용자는 사용자가 검색한 데이터를 사용자 지정하는 입력 값을 제공할 수 있습니다. 자세한 쿼리 또는 보고서를 빌드하는 데 시간을 투자한 후 재작업을 방지하도록 매개 변수를 미리 생각합니다.

> [!NOTE]
> 템플릿 앱은 Any 및 Binary를 제외한 모든 매개 변수를 지원합니다.
>

### <a name="additional-query-tips"></a>추가 쿼리 팁

* 모든 열은 적절하게 형식화되어야 합니다.
* 열에는 정보를 전달하는 이름을 사용합니다([Q&A](#qa) 참조).  
* 공유 논리의 경우 함수 또는 쿼리를 사용하는 것이 좋습니다.  
* 개인 정보 수준은 현재 서비스에서 지원되지 않습니다. 개인 정보 수준에 대한 프롬프트가 표시되면 상대 경로를 사용하도록 쿼리를 다시 작성해야 합니다.  

## <a name="data-models"></a>데이터 모델

잘 정의된 데이터 모델을 통해 고객은 템플릿 앱과 쉽고 직관적으로 상호 작용할 수 있습니다. Power BI Desktop에서 데이터 모델을 생성합니다.

> [!NOTE]
> 대부분의 기본 모델링(형식 지정, 열 이름)은 [쿼리](#queries)에서 수행해야 합니다.

### <a name="qa"></a>질문 및 답변
모델링은 Q&A에서 고객에게 얼마나 잘 결과를 제공할 수 있는지에도 영향을 줍니다. 자주 사용되는 열에 동의어를 추가하고 [쿼리](#queries)에서 열 이름을 적절히 지정했는지 확인합니다.

### <a name="additional-data-model-tips"></a>추가 데이터 모델 팁

다음 사항을 확인하세요.

* 모든 값 열에 서식을 적용했습니다. 쿼리에 형식을 적용합니다.  
* 모든 측정값에 서식을 적용했습니다.
* 기본 요약을 설정합니다. 특히 해당되는 경우(예를 들어 고유 값인 경우) "요약 안 함".  
* 해당되는 경우 데이터 범주를 설정합니다.  
* 필요에 따라 관계를 설정합니다.  

## <a name="reports"></a>보고서
보고서 페이지는 템플릿 앱에 포함된 데이터에 대한 추가 인사이트를 제공합니다. 보고서의 페이지를 사용하여 템플릿 앱에서 해결하려고 하는 주요 비즈니스 질문에 대답할 수 있습니다. Power BI Desktop을 사용하여 보고서를 만듭니다.


### <a name="additional-report-tips"></a>추가 보고서 팁

* 교차 필터링을 위해 페이지당 둘 이상의 시각적 개체를 사용합니다.  
* 시각적 개체를 신중하게 맞춥니다(겹치지 않게).  
* 페이지가 레이아웃을 "4:3" 또는 "16:9" 모드로 설정합니다.  
* 제시된 모든 집계는 숫자로 인식됩니다(평균, 고유 값).  
* 조각화를 통해 관계형 결과가 생성됩니다.  
* 로고는 최상위 보고서 이상에 있습니다.  
* 요소는 클라이언트의 색 구성표에 가능한 범위 내에 있습니다.  

<a name="dashboard"></a>

## <a name="dashboards"></a>대시보드
대시보드는 고객에 대해 템플릿 앱과 상호 작용하는 주요 지점입니다. 포함된 콘텐츠에 대한 개요를 포함하며 특히 비즈니스 시나리오에 대한 중요한 메트릭이 포함됩니다.

템플릿 앱용 대시보드를 만들려면 데이터 가져오기 > 파일을 통해 PBIX를 업로드하거나 Power BI Desktop에서 직접 게시하면 됩니다.


### <a name="additional-dashboard-tips"></a>추가 대시보드 팁

* 고정할 때는 동일한 테마를 유지하여 대시보드의 타일이 일관되게 합니다.  
* 로고를 테마에 고정하여 소비자가 팩의 출처를 알 수 있도록 합니다.  
* 대부분의 화면 해상도에 작동하는 추천 레이아웃은 너비가 5-6개의 작은 타일입니다.  
* 모든 대시보드 타일에는 적절한 제목/부제가 있어야 합니다.  
* 가로 또는 세로로 다양한 시나리오에 맞는 대시보드 그룹화를 사용하는 것이 좋습니다.  

## <a name="sample-data"></a>샘플 데이터
앱 생성 단계의 일부인 템플릿 앱은 작업 영역에 있는 캐시 데이터를 앱의 일부로 래핑합니다.

* 데이터를 연결하기 전에 설치 프로그램이 앱의 기능과 목적을 이해할 수 있게 합니다.
* 설치 프로그램이 앱 데이터 세트 연결로 이어지는 앱 기능을 추가로 탐색하도록 하는 경험을 만듭니다.

앱을 만들기 전에 품질 샘플 데이터를 준비하는 것이 좋습니다. 앱 보고서 및 대시보드가 데이터로 채워졌는지 확인하세요.

## <a name="publishing-on-appsource"></a>AppSource에 게시
템플릿 앱을 AppSource에 게시할 수 있으며, 앱을 AppSource에 제출하기 전에 다음 지침을 따르세요.

* 설치 프로그램이 앱이 수행할 수 있는 기능을 이해하는 데 도움을 줄 수 있는 관련성 있는 샘플 데이터가 포함된 템플릿 앱을 만들어야 합니다(빈 보고서 및 대시보드는 승인되지 않음).
템플릿 앱은 샘플 데이터 전용 앱을 지원하므로, 정적 앱 확인란을 선택해야 합니다. [자세히 알아보기](https://docs.microsoft.com/power-bi/service-template-apps-create#create-the-test-template-app)
* 데이터에 연결하는 데 필요한 자격 증명 및 매개 변수가 포함된 지침을 유효성 검사 팀이 준수하도록 합니다.
* 애플리케이션에는 Power BI 및 제공하는 CPP에 앱 아이콘이 있어야 합니다. [자세히 알아보기](https://docs.microsoft.com/power-bi/service-template-apps-create#create-the-test-template-app)
* 랜딩 페이지를 구성합니다. [자세히 알아보기](https://docs.microsoft.com/power-bi/service-template-apps-create#create-the-test-template-app)
* [Power BI 앱 제품](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/power-bi/cpp-power-bi-offer)에 대한 설명서를 따라야 합니다.
* 대시보드가 앱의 일부인 경우 비어 있지 않은지 확인합니다.
* 앱을 제출하기 전에 앱 링크를 사용하여 앱을 설치합니다. 데이터 세트와 앱 환경을 계획대로 연결할 수 있는지 확인합니다.
* pbix를 템플릿 작업 영역에 업로드하기 전에 불필요한 연결을 언로드해야 합니다.
* 사용자에게 미치는 최대한의 영향을 획득하고 배포 승인을 받을 수 있도록, Power BI [보고서 및 시각적 개체 모범 디자인 사례](https://docs.microsoft.com/power-bi/visuals/power-bi-visualization-best-practices)를 따릅니다.
<!--- * In general, only application with valuable functionality can be approved for general use on AppSource. Application with sample data content only must have either a guidance or statistical value.) -->

## <a name="known-limitations"></a>알려진 제한 사항

| 특정 | 알려진 제한 사항 |
|---------|---------|
|목차:  데이터 세트   | 정확히 하나의 데이터 세트가 있어야 합니다. Power BI Desktop(.pbix 파일)에 기본 제공 데이터 세트만 허용됩니다. <br>지원되지 않음: 다른 템플릿 앱, 작업 영역 간 데이터 세트, 페이지를 매긴 보고서(.rdl 파일), Excel 통합 문서의 데이터 세트 |
|목차: 대시보드 | 실시간 타일은 허용되지 않음(즉, 푸시 또는 스트리밍 데이터 세트에 대한 지원이 없음) |
|목차: 데이터 흐름 | 지원되지 않음: 데이터 흐름 |
|파일의 내용 | PBIX 파일만 허용됩니다. <br>지원되지 않음: .rdl 파일(페이지를 매긴 보고서), Excel 통합 문서   |
| 데이터 원본 | 클라우드 예약 데이터 새로 고침에 대해 지원되는 데이터 원본은 허용됩니다. <br>지원되지 않음: <li> DirectQuery</li><li>라이브 연결(Azure AS 제외)</li> <li>온-프레미스 데이터 원본(개인 및 엔터프라이즈 게이트웨이는 지원되지 않음)</li> <li>실시간(푸시 데이터 세트에 대한 지원 없음)</li> <li>복합 모델</li></ul> |
| 데이터 세트: 작업 영역 간 | 작업 영역 간 데이터 세트는 허용되지 않습니다.  |
| 쿼리 매개 변수 | 지원되지 않음: 데이터 세트를 위한 "Any" 또는 "Binary" 형식 블록 새로 고침 작업의 매개 변수 |
| 사용자 지정 시각적 개체 | 공개적으로 사용할 수 있는 사용자 지정 시각적 개체만 지원됩니다. [조직의 사용자 지정 시각적 개체](developer/power-bi-custom-visuals-organization.md)는 지원되지 않습니다. |

## <a name="next-steps"></a>다음 단계

[Power BI 템플릿 앱이란?](service-template-apps-overview.md)
