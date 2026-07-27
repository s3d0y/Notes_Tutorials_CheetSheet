# Harness engineering — шпаргалка

Источник: [Что такое Harness? (Habr, 2026)](https://habr.com/ru/articles/1023316/)

Раздел: [[../00_MOC/MOC — Инструменты и метанавыки]] · [[../MOC — Notes_Tutorials_CheetSheet]]

---

## Одной строкой

**Agent harness** — вся инфраструктура вокруг LLM, которая превращает stateless-модель в работающего агента.

> «Если ты не модель — ты harness.» (Vivek Trivedi, LangChain)

**Агент** = поведение (целенаправленный, с инструментами, самокорректирующийся).  
**Harness** = механизм, который это поведение производит.

Аналогия: LLM = CPU; контекстное окно = RAM; внешние БД = диск; tools = драйверы; **harness = ОС**.

---

## Три уровня инжиниринга

1. **Prompt engineering** — что сказать модели.
2. **Context engineering** — что модель видит и когда.
3. **Harness engineering** — всё остальное: оркестрация, память, ошибки, безопасность, lifecycle.

---

## 12 компонентов production-harness (кратко)

| # | Компонент | Суть |
|---|-----------|------|
| 1 | **Оркестрационный цикл** | ReAct / TAO: prompt → LLM → parse → tools → observe → repeat |
| 2 | **Tools** | Схемы, валидация, выполнение, форматирование наблюдений |
| 3 | **Memory** | Краткая (сессия) + долгая (между сессиями) |
| 4 | **Context management** | Compaction, pruning, «lost in the middle», context rot |
| 5 | **State / persistence** | Чекпоинты, resume, thread id |
| 6 | **Error handling** | Retry, fallback, graceful degradation |
| 7 | **Verification** | Тесты, линтеры, human-in-the-loop |
| 8 | **Security** | Sandboxing, permissions, prompt injection defense |
| 9 | **Subagents** | Делегирование подзадач |
| 10 | **MCP / integrations** | Внешние системы, стандартизированные коннекторы |
| 11 | **Observability** | Логи, трейсы, метрики, eval |
| 12 | **Deployment** | SDK, CLI, API, CI для harness |

*(полный разбор — в статье на Habr)*

---

## Примеры harness в индустрии

- **Claude Code** (Anthropic) — SDK как harness
- **Codex / Agents SDK** (OpenAI)
- **LangGraph / LangChain**
- **Cursor** — harness вокруг coding-агента

---

## Зачем это тебе (PM / продукт / ДорХаб)

- Отличать «купили модель» от «построили систему».
- В разговорах про AI-фичи: **где продукт, где harness, где модель**.
- При оценке конкурентов и пилотов: не только «есть GPT», а **цикл, tools, память, контроль контекста**.

---

## Что изучить дальше

- [ ] Прочитать статью целиком + выписать 3 идеи для своих проектов
- [ ] Сопоставить с тем, как устроен **Cursor** / **Claude Code**
- [ ] Понять разницу **prompt vs context vs harness** на примере одной задачи (например, charter пилота)
