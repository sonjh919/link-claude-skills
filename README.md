# link-claude-skills

Claude Code 작업 개선 스킬 모음. 작업 세션을 돌아보고 더 나은 방법을 제안합니다.

## 설치

### Plugin으로 설치 (권장)

Claude Code에서 다음 명령어를 실행합니다:

```
/plugin marketplace add sonjh919/link-claude-skills
```

원하는 스킬을 설치합니다:

```
/plugin install tips@link-claude-skills
/plugin install skillify@link-claude-skills
```

### 수동 설치

프로젝트의 `.claude/skills/` 디렉토리에 직접 복사합니다:

```bash
# tips 스킬
mkdir -p .claude/skills/tips
curl -o .claude/skills/tips/SKILL.md \
  https://raw.githubusercontent.com/sonjh919/link-claude-skills/master/skills/tips/SKILL.md

# skillify 스킬
mkdir -p .claude/skills/skillify
curl -o .claude/skills/skillify/SKILL.md \
  https://raw.githubusercontent.com/sonjh919/link-claude-skills/master/skills/skillify/SKILL.md
```

## 스킬 목록

| 스킬 | 설명 | 출력 |
|------|------|------|
| `/tips` | 활용하지 못한 Claude Code 기능을 분석하여 개선 제안 | `tips-YYYY-MM-DD.md` |
| `/skillify` | 반복된 작업 패턴을 찾아 커스텀 스킬화 추천 | `skillify-YYYY-MM-DD.md` |

모든 스킬은 **작업이 끝난 후** 호출하는 것이 가장 효과적입니다.

## 요구사항

- Claude Code CLI
- Claude Code Plugin 기능 지원 버전

## 라이선스

MIT
