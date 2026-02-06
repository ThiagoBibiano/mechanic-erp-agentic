# 01 - Manifesto do MVP

## Objetivo

Garantir um MVP operavel para oficina mecanica, com foco no ciclo completo de Ordem de Servico (OS):
recepcao, execucao e fechamento com reflexos transacionais padrao do ERPNext.

## Fluxo Minimo Operacional

1. Recepcao
- Buscar cliente e veiculo por chave rapida (placa, telefone, documento, chassi).
- Se nao existir, cadastrar cliente e veiculo com vinculo obrigatorio.
- Abrir OS com checklist inicial e dados de entrada.

2. Execucao
- Registrar diagnostico (texto objetivo + checklist tecnico).
- Lancar pecas e servicos na OS.
- Atualizar status da OS ate condicao de fechamento.

3. Fechamento
- Validar pre-condicoes da OS.
- Finalizar OS e disparar reflexos padrao (estoque e faturamento).
- Gerar documentos ERPNext previstos para o processo.
- Disponibilizar comprovante interno para impressao/compartilhamento.

## Inclui (MVP)

- Cadastro de `Customer` e `Veiculo` com vinculo obrigatorio.
- Ciclo de vida de `Ordem de Servico` com estados padronizados.
- Lancamento de `Itens` classificados como `PECA` e `SERVICO`.
- Warehouses minimos com separacao entre fluxo de peca e fluxo de servico.
- Finalizacao da OS com automacao minima de estoque e faturamento.
- Interface mobile-first para recepcao e mecanico nos fluxos criticos.
- MCP com tools essenciais:
  - historico de cliente/veiculo
  - rascunho de OS
  - disponibilidade de peca
- RAG tecnico basico somente se houver base minima curada.

## Exclui (Pos-MVP)

- Conciliacao financeira avancada e fiscal complexo.
- Integracoes externas amplas (contabilidade, gateways, parceiros).
- CRM completo, funis e campanhas.
- BI executivo avancado.
- Omnichannel/WhatsApp nao critico para operacao minima.
- Modelos sofisticados de previsao e otimizacao.

## O que significa "MVP pronto"

O MVP esta pronto quando:

- A oficina consegue abrir, executar e fechar OS sem depender do Desk padrao.
- A finalizacao da OS gera reflexo transacional consistente.
- O time operacional recebe mensagens de erro orientadas a acao.
- O fluxo cobre os casos frequentes sem retrabalho manual excessivo.

