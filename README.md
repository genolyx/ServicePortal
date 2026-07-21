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
