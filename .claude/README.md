# Konfiguracja Claude Code

`../CLAUDE.md` jest automatycznie ładowanym, wspólnym kontekstem projektu. Import `@.claude/starting-prompt.md` dołącza pełną treść promptu startowego do kontekstu każdej nowej sesji Claude Code rozpoczętej w tym repozytorium. Plik nie uruchamia się jako osobna komenda — jest ładowany jako część instrukcji projektu.

## Struktura

- `settings.json` — wspólne ustawienia projektu; obecnie pusty obiekt, bez nadawania dodatkowych uprawnień.
- `templates/agent.md` — wzór wyspecjalizowanego subagenta. Po uzupełnieniu kopiuj go do `agents/<nazwa>.md`.
- `templates/skill/SKILL.md` — wzór umiejętności/workflow. Po uzupełnieniu kopiuj katalog do `skills/<nazwa>/SKILL.md`.

Dodawaj agentów i skills dopiero wtedy, gdy pojawi się powtarzalny, konkretny workflow. Nie wpisuj do ustawień stałej zgody na polecenia ani dostępy, zanim nie będzie jasne, jakie są potrzebne. Ustawienia osobiste trzymaj poza wspólnym `settings.json`.

Claude Code obsługuje komendy slash jako skills; nowej komendy nie trzeba tworzyć osobno, jeśli może być opisana jako skill.
