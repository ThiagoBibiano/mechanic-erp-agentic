# 02 - Jornadas por Persona

## Personas operacionais

### Recepcionista / Atendimento

Permissoes e foco:

- Buscar cliente e veiculo rapidamente.
- Cadastrar cliente/veiculo quando necessario.
- Abrir OS e preencher checklist de entrada.
- Coletar fotos, observacoes e assinatura (quando aplicavel).

Jornada feliz:

1. Busca cliente/veiculo por placa ou telefone.
2. Abre OS com queixa principal e checklist de entrada.
3. Encaminha OS para triagem/execucao.

Jornada nao feliz (excecoes comuns):

- Cliente sem cadastro: cria `Customer` e prossegue.
- Veiculo sem placa (caso especial): usa chassi + identificador interno.
- Dados incompletos: bloqueia avancar ate obrigatorios minimos.

### Mecanico

Permissoes e foco:

- Visualizar fila de OS atribuidas.
- Atualizar status conforme andamento.
- Registrar diagnostico, itens e observacoes tecnicas.
- Consultar assistente tecnico (MCP/RAG), se habilitado.

Jornada feliz:

1. Assume OS em triagem.
2. Registra diagnostico e plano de servico.
3. Lanca pecas/servicos e atualiza status.
4. Marca OS como pronta para fechamento.

Jornada nao feliz (excecoes comuns):

- Peca sem estoque: sinaliza indisponibilidade e aciona tratamento.
- Divergencia de item/servico: ajusta lancamento antes de avancar.
- Retrabalho/garantia: abre ocorrencia vinculada ao historico da OS.

### Gestor / Backoffice

Permissoes e foco:

- Validar excecoes e bloqueios.
- Auditar alteracoes sensiveis (status, itens, valores).
- Aplicar ajustes pontuais e liberar fluxo quando necessario.

Jornada feliz:

1. Monitora OS bloqueadas/pendentes.
2. Autoriza excecoes previstas em regra.
3. Fecha pendencias operacionais.

Jornada nao feliz (excecoes comuns):

- OS cancelada: valida motivo e encerra trilha de auditoria.
- OS parada em aprovacao: decide aprovar/reprovar com justificativa.

## Mapa simples de entradas e saidas

- Entrada principal: demanda de servico com cliente e veiculo identificados.
- Saida principal: OS finalizada com reflexos de estoque/faturamento.
- Excecoes frequentes: cadastro ausente, estoque indisponivel, cancelamento, garantia e divergencia de lancamento.

