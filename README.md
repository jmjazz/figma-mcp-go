---
name: figma-setup
description: figma-mcp-go 연결 상태를 점검하고, 플러그인 셋업을 안내합니다. "figma 연결", "figma setup", "figma-mcp 안 됨", "/figma-setup" 등의 상황에서 사용하세요.
---

# Figma MCP Go — 연결 점검 & 셋업 가이드

이 스킬이 실행되면 아래 순서로 진행합니다.

---

## Step 1 — 연결 상태 점검

`mcp__figma-mcp-go__get_variable_defs` 또는 `mcp__figma-mcp-go__whoami` 등
figma-mcp-go 도구를 호출해 현재 연결 상태를 확인합니다.

- 연결 OK → "figma-mcp-go 연결 정상입니다." 메시지 출력 후 종료
- 연결 실패 / 도구 없음 → Step 2로 이동

---

## Step 2 — 상태 진단 & 안내

연결이 안 된 경우, 아래 순서로 원인을 확인하고 안내합니다.

### A. MCP 서버가 등록되지 않은 경우 (최초 1회)

프로젝트 루트에 .mcp.json 파일을 생성하고 아래 내용을 추가합니다:

  {
    "mcpServers": {
      "figma-mcp-go": {
        "command": "npx",
        "args": ["-y", "@vkhanhqui/figma-mcp-go"]
      }
    }
  }

Claude Code에서 /mcp 입력 → figma-mcp-go 항목이 보이면 등록 완료.
Cursor / VS Code는 .vscode/mcp.json에 동일하게 추가합니다.

### B. 플러그인 최초 import를 아직 안 한 경우 (최초 1회)

1. Figma 데스크톱 앱 실행 (브라우저 버전 X)
2. GitHub Releases에서 plugin.zip 다운로드 후 압축 해제
   https://github.com/vkhanhqui/figma-mcp-go/releases
3. Plugins → Development → Import plugin from manifest…
4. 압축 해제한 폴더 안의 manifest.json 선택
5. Import 완료 → 계정에 등록됨 (다음부터 이 단계 불필요)

### C. 플러그인을 실행하지 않은 경우 (매 작업 시)

1. 변수가 있는 Figma 파일 열기
   ※ 변수가 별도 DS 라이브러리 파일에 있으면 그 라이브러리 파일을 열어야 합니다
2. Plugins → Development → Figma MCP Go 실행
3. 플러그인 창을 열어둔 채 유지 (닫으면 브릿지 끊김)
4. 이후 Claude에서 도구 재시도

---

## Step 3 — 재확인

안내 완료 후, 사용자가 준비됐다고 하면 도구를 다시 호출해 연결을 확인합니다.

---

## 참고

- 서버 연결 주소: ws://127.0.0.1:1994
- 서버 버전 확인 (터미널):
  echo '{"jsonrpc":"2.0","id":1,...}' | npx -y @vkhanhqui/figma-mcp-go@latest
- 자세한 내용: https://github.com/vkhanhqui/figma-mcp-go