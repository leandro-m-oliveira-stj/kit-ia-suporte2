# Instruções para agentes de IA

## Divisão de trabalho

| Papel | Modelo | Faz | Não faz |
|---|---|---|---|
| Coordenador e revisor | Opus | Planeja, delega, revisa o diff, decide o commit | Escrever código; rodar testes de mutação, de carga ou outras suítes pesadas |
| Programador | Sonnet, esforço médio | Escreve o código e roda os testes, inclusive os pesados | Fazer commit sem a revisão do Opus |

Fluxo:
1. O Opus descreve a tarefa e a entrega a um subagente Sonnet (`model: sonnet`, `effort: medium`).
2. O Sonnet implementa, testa e devolve o diff com o resultado dos testes.
3. O Opus revisa. Se houver problema, devolve ao mesmo subagente para corrigir.
4. Só com a revisão aprovada o código é commitado.

## Git

- Com autorização do usuário, envie direto para a `main` (push, sem pull request nem merge).
- Mudanças simples (texto, documentação, ajuste pequeno e revisado no painel) podem ir para a `main` sem perguntar.
- Nas demais, trabalhe na branch da sessão e pergunte antes de levar para a `main`.

## Estilo e Clean Code

- **Clareza acima de esperteza:** Escreva código legível, idiomático e com nomes que revelem intenção. Siga os padrões já existentes no repositório.
- **Funções pequenas e coesas:** Cada função/método deve ter uma única responsabilidade. Prefira *early returns* (cláusulas de guarda) para evitar aninhamento excessivo.
- **YAGNI e KISS:** Não crie abstrações prematuras, *wrappers* desnecessários ou flexibilidade especulativa. Resolva o problema de forma direta.
- **Tratamento de erros explícito:** Valide entradas nas fronteiras e falhe rápido com contexto claro. Nunca silencie exceções, exceto quando houver motivação forte (explique-a num comentário).
- **Comentários úteis:** Comente apenas o *porquê* de decisões não óbvias, nunca *o quê* o código faz. Não deixe código morto, imports não usados ou logs de debug.

## Projeto

- Idioma: português do Brasil em textos, mensagens de commit e comentários.
- Estrutura: `bot/` (apps do Dify), `servicos/app/` (painel, conector e APIs simuladas em Python/FastAPI), `base-de-conhecimento/`, `dify/`, `chatwoot/`.
- `config/` guarda chaves e estado local: nunca vai para o git.
- Para aplicar mudanças no Codespace: `git pull origin main && cd servicos && docker compose up -d --build`.
