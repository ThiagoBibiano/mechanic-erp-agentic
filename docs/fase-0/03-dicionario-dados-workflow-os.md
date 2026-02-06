# 03 - Dicionario de Dados e Workflow da OS

## Entidades minimas e responsabilidades

### Customer (Cliente)

Responsabilidade:

- Entidade pagadora (pessoa fisica ou juridica).
- Pode possuir multiplos veiculos.

Campos obrigatorios:

- `customer_name`
- `customer_type` (`Individual` ou `Company`)
- `mobile_no` ou `phone`
- `tax_id` (CPF/CNPJ quando exigido pela politica local)

Chaves e busca:

- Unicidade por `tax_id` quando preenchido.
- Busca por `customer_name`, `mobile_no`, `tax_id`.

### Veiculo

Responsabilidade:

- Representar o ativo atendido pela oficina.
- Manter historico tecnico por OS.

Campos obrigatorios:

- `customer` (vinculo obrigatorio com Customer)
- `plate` ou `chassis` (ao menos um obrigatorio)
- `brand`
- `model`
- `model_year`
- `odometer_km` (na entrada da OS)

Chaves e busca:

- Unicidade preferencial por `plate`.
- Fallback por `chassis` quando sem placa.
- Busca composta por `plate`, `chassis`, `customer`.

### Ordem de Servico (OS)

Responsabilidade:

- Caso operacional do atendimento.
- Agregador de diagnostico, checklist, itens, midia e historico de status.

Campos obrigatorios:

- `customer`
- `vehicle`
- `opened_at`
- `complaint_summary`
- `status`
- `assigned_to` (quando sair de "Aberta")

Chaves e busca:

- Identificador unico `os_number` (serie padrao).
- Busca por `os_number`, `plate`, `customer`, `status`, `assigned_to`.

### Item (PECA ou SERVICO)

Responsabilidade:

- Componente de cobranca/execucao dentro da OS.

Campos obrigatorios:

- `item_code`
- `item_type` (`PECA` ou `SERVICO`)
- `qty`
- `rate`
- `warehouse` (obrigatorio para `PECA`)

Regras de classificacao:

- `PECA`: governada por estoque e warehouse.
- `SERVICO`: sem estoque, com precificacao e contabilizacao.

### Warehouse

Responsabilidade:

- Local logico de controle de estoque e movimentacao.

Estrutura minima:

- `LOJA` (estoque comercial)
- `OFICINA` (consumo operacional)
- `SUCATA` (descartes e retornos improprios)

## Padroes de formatacao

- Placa: uppercase, sem espacos extras, aceitar Mercosul.
- Telefone: formato E.164 quando possivel, armazenar somente digitos no normalizado.
- CPF/CNPJ: armazenar sem mascara no campo tecnico.
- Quilometragem: inteiro positivo, unidade em km.

## Workflow padrao da OS

Estados:

1. `ABERTA`
2. `EM_TRIAGEM`
3. `AGUARDANDO_APROVACAO` (opcional por politica da oficina)
4. `EM_EXECUCAO`
5. `PRONTA_PARA_FECHAMENTO`
6. `FINALIZADA`
7. `CANCELADA`

Regras por estado:

| Estado | Permite | Bloqueia | Obrigatorio para avancar |
|---|---|---|---|
| `ABERTA` | editar dados iniciais, anexar checklist/fotos | fechamento | cliente, veiculo, queixa principal |
| `EM_TRIAGEM` | registrar diagnostico preliminar, atribuir mecanico | finalizacao | mecanico responsavel |
| `AGUARDANDO_APROVACAO` | revisar orcamento e aprovar/reprovar | execucao sem aprovacao (se regra ativa) | decisao de aprovacao |
| `EM_EXECUCAO` | lancar itens e atualizar observacoes | cancelamento sem motivo | diagnostico, itens coerentes |
| `PRONTA_PARA_FECHAMENTO` | validar totais e pre-condicoes | edicao estrutural de itens sem reabrir | checklist final, totais validos |
| `FINALIZADA` | consulta e auditoria | alteracao operacional direta | reflexos transacionais concluidos |
| `CANCELADA` | consulta e auditoria | qualquer reflexo de faturamento/estoque | motivo de cancelamento |

Definicoes finais:

- `Finalizacao`: conclusao operacional com disparo de reflexos de estoque/faturamento previstos.
- `Cancelamento`: encerramento sem reflexo financeiro final, exigindo motivo e trilha de auditoria.

