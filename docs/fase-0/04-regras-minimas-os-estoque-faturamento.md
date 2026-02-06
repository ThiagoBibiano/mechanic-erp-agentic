# 04 - Regras Minimas de OS, Estoque e Faturamento

## Regras de OS

- A OS so pode ser aberta com `customer` e `vehicle` validos.
- A OS deve nascer em `ABERTA` com `complaint_summary`.
- Se a oficina usar aprovacao, a execucao plena exige passagem por `AGUARDANDO_APROVACAO`.
- Quilometragem de referencia da OS e capturada na abertura e pode ser atualizada somente por perfil autorizado.
- Reabertura de OS finalizada e excecao:
  - permitida apenas por gestor
  - exige justificativa
  - registra auditoria da reabertura

Por que:

- Evita dados incompletos e retrabalho.
- Preserva trilha operacional e evita alteracoes silenciosas apos fechamento.

Impacto no fluxo:

- Menos inconsistencias em historico de veiculo.
- Menos bloqueios no fechamento por ausencia de dados criticos.

## Regras de estoque e itens

- Item `PECA` exige warehouse valido e disponibilidade conforme politica.
- Baixa/movimentacao de `PECA` ocorre na finalizacao da OS (MVP padrao).
- Item `SERVICO` nao movimenta estoque.
- `SERVICO` exige `rate` e unidade de cobranca coerente (`hora` ou `servico fechado`).
- Movimentacoes permitidas no MVP:
  - `LOJA -> OFICINA` (consumo operacional)
  - `OFICINA -> SUCATA` (perda/descartes tecnicos)

Por que:

- Simplifica contabilizacao no MVP e reduz chance de divergencia entre operacao e estoque.

Impacto no fluxo:

- Fechamento com reflexo previsivel e audivel.
- Tratamento explicito para peca sem estoque antes de concluir OS.

## Regras de faturamento minimo

- O fechamento da OS gera documento de faturamento previsto no ERPNext.
- Estrategia MVP:
  - gerar documento em rascunho automaticamente
  - emissao final por conferente/gestor conforme rotina da oficina
- Desconto simples permitido com limite percentual configuravel.
- Ajustes de valor apos finalizacao exigem processo de reabertura controlada.

Por que:

- Garante velocidade operacional sem perder controle financeiro.

Impacto no fluxo:

- Equipe operacional fecha OS sem etapa fiscal avancada.
- Backoffice mantem governanca sobre emissao definitiva.

