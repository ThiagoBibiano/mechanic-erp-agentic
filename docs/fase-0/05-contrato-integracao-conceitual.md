# 05 - Contrato Conceitual de Integracao

## Objetivo

Definir um contrato estavel para consumo por UI (mobile-first/SSR) e MCP tools,
antes da implementacao de API definitiva.

## Operacoes expostas (conceitual)

1. Buscar cliente/veiculo
- Entrada: `query` (placa, telefone, documento, chassi), filtros opcionais.
- Saida: lista curta com identificadores e resumo operacional.

2. Abrir OS
- Entrada: `customer_id`, `vehicle_id`, `complaint_summary`, checklist inicial.
- Saida: `os_id`, `os_number`, `status`.

3. Listar OS por usuario
- Entrada: `user_id`, `status[]`, paginacao.
- Saida: fila operacional ordenada por prioridade/abertura.

4. Lancar item na OS
- Entrada: `os_id`, `item_code`, `item_type`, `qty`, `rate`, `warehouse` (se peca).
- Saida: item registrado + totais atualizados da OS.

5. Finalizar OS
- Entrada: `os_id`, confirmacoes de checklist final.
- Saida: status final + referencias de documentos gerados.

6. Consultar estoque/disponibilidade
- Entrada: `item_code`, `warehouse` opcional.
- Saida: saldo, reservado, disponivel, previsao de reposicao (se houver).

## Padrao de resposta

Formato de sucesso:

```json
{
  "ok": true,
  "data": {},
  "meta": {
    "request_id": "uuid",
    "timestamp": "ISO-8601"
  }
}
```

Formato de erro:

```json
{
  "ok": false,
  "error": {
    "type": "VALIDATION_ERROR|BUSINESS_RULE|NOT_FOUND|CONFLICT|INTERNAL",
    "code": "STRING_ESTAVEL",
    "message": "Mensagem curta orientada a acao",
    "action": "Proximo passo recomendado ao operador"
  },
  "meta": {
    "request_id": "uuid",
    "timestamp": "ISO-8601"
  }
}
```

## Validacoes padrao

- Campo faltante: retornar `VALIDATION_ERROR` com lista objetiva.
- Estado invalido da OS: retornar `BUSINESS_RULE` com estado atual e transicao esperada.
- Conflito de concorrencia: retornar `CONFLICT` com orientacao de recarga da OS.

## Politica de mensagens e erros

- Frases curtas e operacionais.
- Sem jargao tecnico para perfis de oficina.
- Sempre incluir acao recomendada quando recuperavel.

Classificacao:

- Erro recuperavel: operador consegue corrigir dado/estado e seguir.
- Erro bloqueante: exige perfil autorizado ou intervencao tecnica.

## Auditoria e rastreabilidade minima

Registrar obrigatoriamente:

- Mudancas de status da OS (`from`, `to`, `who`, `when`, `reason` quando houver).
- Alteracoes de itens e valores (antes/depois).
- Acao de finalizacao/cancelamento com usuario e timestamp.
- Correlacao por `request_id` para suporte e depuracao.

