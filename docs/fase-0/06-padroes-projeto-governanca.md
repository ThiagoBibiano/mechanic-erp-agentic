# 06 - Padroes de Projeto e Governanca

## Estrutura canonica do projeto

Estrutura alvo para execucao solo:

```text
apps/
  erp_oficina/
    erp_oficina/
      doctype/
      api/
      services/
      workflows/
      validations/
mcp/
  server/
  tools/
  prompts/
web/
  templates/
  static/
docs/
  fase-0/
  decisions/
  prompts/
scripts/
tests/
```

Diretrizes:

- `erp_oficina` contem regra de negocio e integracao com ERPNext/Frappe.
- `mcp` contem servidor e tools operacionais.
- `web/templates` contem experiencia SSR/mobile-first.
- `docs/decisions` concentra decisoes arquiteturais curtas.

## Convencoes de nomenclatura

DocTypes e campos:

- DocTypes de dominio em ingles tecnico curto: `ServiceOrder`, `Vehicle`.
- Labels de UI podem ser em portugues.
- Campos em `snake_case`.
- Campo de integracao com `external_` prefixado apenas quando realmente externo.

Estados e workflows:

- Estados de OS em `UPPER_SNAKE_CASE`.
- Transicoes nomeadas por verbo de acao: `start_triage`, `approve_budget`, `finalize_order`.

Endpoints e recursos:

- Prefixo de API: `/api/erp_oficina/v1/`.
- Recursos no plural: `/service-orders`, `/vehicles`.
- Acoes explicitas quando nao CRUD puro: `/service-orders/{id}:finalize`.

Tools MCP:

- Nome curto orientado a tarefa: `os_create_draft`, `os_get_history`, `stock_check_availability`.
- Parametros estaveis e documentados em markdown no proprio repositorio.

## Versionamento de decisoes e prompts

Decisoes arquiteturais:

- Registrar em `docs/decisions/ADR-XXXX-titulo-curto.md`.
- Cada ADR contem contexto, decisao, impacto e status.

Prompts e regras de agente:

- Armazenar em `docs/prompts/`.
- Versionar por arquivo com sufixo semantico: `base-v1.md`, `base-v2.md`.
- Toda mudanca relevante deve mencionar:
  - objetivo operacional
  - risco esperado
  - criterio de reversao

Mudancas que afetam operacao:

- Registrar em `CHANGELOG.md` (secao `Operational Impact`).
- Incluir data, modulo afetado e acao requerida para usuarios internos.

## Rotina minima de desenvolvimento solo

- Cada tarefa muda no maximo um fluxo principal por vez.
- Antes de codar: validar aderencia ao Manifesto MVP.
- Antes de merge local: executar cenarios de validacao definidos na Fase 0.
- Sem nova feature fora do MVP sem atualizar `inclui/exclui` formalmente.

