# 07 - Validacao e Criterios de Aceite

## Cenarios de teste do fluxo minimo

### Cenario 1 - Novo cliente e novo veiculo ate fechamento

Passos:

1. Cadastrar `Customer` novo.
2. Cadastrar `Veiculo` novo vinculado.
3. Abrir OS com checklist inicial.
4. Lancar ao menos 1 servico e 1 peca.
5. Finalizar OS.

Resultado esperado:

- OS transita sem erro de estado.
- Reflexos transacionais sao gerados conforme regra MVP.

### Cenario 2 - Cliente existente + peca sem estoque

Passos:

1. Buscar cliente/veiculo existente.
2. Abrir nova OS.
3. Lancar peca sem disponibilidade.
4. Aplicar tratamento previsto (substituicao, pendencia ou bloqueio).

Resultado esperado:

- Sistema nao permite fechamento inconsistente.
- Mensagem orienta acao clara para continuar.

### Cenario 3 - OS cancelada ou em aprovacao

Passos:

1. Abrir OS e avancar ate estado intermediario.
2. Encaminhar para aprovacao (se ativo) ou cancelar com motivo.

Resultado esperado:

- Cancelamento exige justificativa e registra auditoria.
- Estado de aprovacao bloqueia avancos indevidos.

### Cenario 4 - OS com fotos e checklist

Passos:

1. Abrir OS com checklist de entrada completo.
2. Anexar fotos.
3. Seguir fluxo ate fechamento.

Resultado esperado:

- Midias e checklist ficam vinculados ao historico da OS.
- Consulta posterior recupera evidencias sem ambiguidade.

## Criterios de aceite por macrofluxo

Cadastro:

- Cadastro de cliente/veiculo ocorre sem retrabalho.
- Regras de obrigatoriedade e unicidade estao ativas.

Execucao de OS:

- OS percorre estados definidos sem buracos de permissao.
- Itens `PECA` e `SERVICO` respeitam regras distintas.

Fechamento:

- Finalizacao da OS aciona reflexos automatizados previstos.
- Operador recebe confirmacao objetiva do encerramento.

Operacao mobile-first:

- Fluxos criticos rodam sem dependencia do Desk padrao.
- Tempo de abertura e atualizacao de OS e adequado para uso em oficina.

Mensagens e validacoes:

- Erros sao acionaveis e sem jargao tecnico.
- Distincao clara entre erro recuperavel e bloqueante.

## Definicao de pronto da Fase 0 (Gate)

A Fase 0 pode ser encerrada quando todos os itens abaixo estiverem verdadeiros:

- Manifesto MVP publicado com inclui/exclui e fluxo minimo.
- Jornadas por persona publicadas com excecoes recorrentes.
- Dicionario de dados, chaves e workflow da OS definidos.
- Regras minimas de OS/estoque/faturamento registradas.
- Contrato conceitual de integracao documentado.
- Padroes e governanca de desenvolvimento definidos.
- Cenarios e criterios de aceite formalizados.

