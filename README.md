# ServicePortal

Genolyx 분석 데몬 연결용 단일 파일 웹 포털.

## 파일 구조

```
ServicePortal/
└── index.html   # 전체 UI (CSS/JS 인라인, 외부 의존성 없음)
```

## 사용 방법

### 브라우저에서 직접 열기
```
file:///home/ken/ServicePortal/index.html
```

### 간단한 HTTP 서버로 서빙 (선택)
```bash
cd /home/ken/ServicePortal
python3 -m http.server 8080
# → http://localhost:8080
```

## GX Portal 연동

ServicePortal + gx-daemon이 문서의 **외부 Portal**(분석·리뷰·PDF) 역할입니다.

GX가 호출하는 API는 **gx-daemon**에 있습니다.

| 메서드 | 경로 | 역할 |
|---|---|---|
| `GET` | `/v1/order-schema?service_code=CARRIER` | 오더 필드 스키마 |
| `POST` | `/v1/orders` | GX `order_id`로 주문 생성, FASTQ URL 즉시 다운로드 후 파이프라인 시작 |
| `POST` | `/v1/orders/{order_id}/send-report` | ServicePortal Send — 최신 `Report_*.pdf`를 `callback.report_url`로 전송 |

인증: `Authorization: Bearer {GX_EXTERNAL_API_KEY 또는 API_KEY}`

GX로 PDF를 보낼 때: daemon `.env`에 `GX_CALLBACK_API_KEY` (GX가 발급한 inbound 키)

오더 리스트에 **GX** 뱃지가 보이면 ⋯ 메뉴에서 **Send to GX Portal**을 사용합니다.

지원 `service_code`: `CARRIER`, `WHOLE_EXOME`, `HEALTH_SCREENING`, `SGNIPT`

## 데몬 연결

Configuration 탭에서 연결 대상 선택:

| 프리셋 | 포트 | 용도 |
|---|---|---|
| gx-daemon (prod) | :8010 | 운영 |
| gx-daemon (dev) | :8011 | 개발/테스트 |
| service-daemon | :8003 | 로컬 서비스 |
| nipt-daemon | :8000 | NIPT 전용 |

연결 설정은 브라우저 `localStorage`에 저장됩니다.

## service-daemon 포털과의 관계

- `/home/ken/service-daemon/portal/index.html` 에서 분리된 독립 프로젝트
- service-daemon에 의존하지 않으며, 어느 데몬과도 독립적으로 연결 가능
- gx-daemon 테스트에 최적화
