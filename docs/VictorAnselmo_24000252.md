# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

Aluno: Victor Anselmo Rodrigues  
RA: 24000252  
Data: 26/03/2025 

---

1. Definição do MVP
O MVP foca no Fluxo Essencial de Operação de Balcão e Ciclo de Inventário. O objetivo é garantir que a farmácia possa vender, baixar o estoque em tempo real e registrar a entrada de receita, eliminando a divergência de dados manual.

Dentro do MVP:

Cadastro e consulta de produtos e clientes.

Fluxo de venda completo (À vista e A prazo).

Atualização automática de estoque pós-venda.

Módulo básico de Contas a Receber (vendas a prazo).

Relatório de fechamento de caixa diário e nível crítico de estoque.

Fora do MVP:

Módulo completo de Compras e Contas a Pagar (será feito via ajuste de estoque manual/gerencial nesta fase).

Transferência entre unidades.

Gestão de receitas controladas (SNGPC).

Integração com convênios externos (PBM).

Justificativa: Sem um controle de venda e estoque confiável, nenhum relatório financeiro ou de compras será preciso. Resolver o balcão é a prioridade zero.

2. Regras de Negócio (RN)
RN01 (Estoque Mínimo): O sistema deve impedir a finalização de uma venda caso a quantidade solicitada seja superior ao saldo em estoque da unidade.

RN02 (Venda a Prazo): Somente clientes com cadastro completo e status "Ativo" podem realizar compras na modalidade a prazo.

RN03 (Preço de Venda): O sistema não deve permitir a alteração manual do preço unitário do produto pelo atendente no momento da venda (apenas gerentes).

RN04 (Identificação de Medicamentos): Todo medicamento vendido deve estar obrigatoriamente vinculado a um fabricante e possuir código de barras único.

RN05 (Atualização de Saldo): Toda venda finalizada deve abater instantaneamente a quantidade do estoque da unidade local.

3. Requisitos Funcionais (RF)
RF01: O sistema deve permitir o cadastro de produtos com descrição, preço, fabricante e unidade de medida.

RF02: O sistema deve realizar a busca de produtos por nome ou código de barras.

RF03: O sistema deve permitir o cadastro rápido de clientes (Nome, CPF/CNPJ, Telefone).

RF04: O sistema deve registrar vendas selecionando múltiplos itens e calculando o subtotal.

RF05: O sistema deve processar pagamentos em dinheiro, cartão e "a prazo" (conta cliente).

RF06: O sistema deve emitir um comprovante de venda não fiscal (resumo da operação).

RF07: O sistema deve gerar alerta visual quando o estoque de um item atingir o nível mínimo definido.

RF08: O sistema deve registrar lançamentos automáticos no "Contas a Receber" para vendas a prazo.

4. Requisitos Não Funcionais (RNF)
RNF01 (Disponibilidade): O sistema deve estar disponível 99,5% do tempo durante o horário comercial.

RNF02 (Tempo de Resposta): A consulta de produtos no banco de dados deve retornar resultados em menos de 2 segundos.

RNF03 (Segurança): O acesso deve ser restrito por login e senha, com perfis de permissão (Atendente vs Gerente).

RNF04 (Integridade): O sistema deve utilizar transações de banco de dados para garantir que a baixa no estoque só ocorra se a venda for confirmada.

5. Casos de Uso:

UC01: Efetuar Venda (Atendente)

UC02: Consultar Produto (Atendente) - << include >> em UC01

UC03: Cadastrar Cliente (Atendente) - << extend >> em UC01

UC04: Verificar Estoque (Sistema) - << include >> em UC01

UC05: Registrar Conta a Receber (Sistema) - << extend >> em UC01 (se for a prazo)

UC06: Manter Produto (Gerente)

UC07: Ajustar Estoque Manualmente (Gerente)

UC08: Gerar Relatório de Vendas (Gerente/Financeiro)

UC09: Autenticar Usuário (Todos os Atores) - << include >> em UC01, UC06, UC08

UC10: Emitir Comprovante (Sistema) - << include >> em UC01

6. Documentação de Caso de Uso

   
#UC01 — Efetuar Venda

Ator(es): Atendente.

Descrição: Permite registrar a saída de produtos para um cliente e processar o pagamento.

Pré-condições: Usuário autenticado; Estoque disponível.

Pós-condições: Estoque atualizado; Venda registrada; Comprovante emitido.

Fluxo Principal:

Atendente inicia a venda.

Atendente busca e adiciona produtos (UC02).

Sistema valida estoque (UC04).

Sistema exibe o total.

Atendente seleciona forma de pagamento.

Sistema finaliza a venda e gera comprovante (UC10).

Fluxos Alternativos / Exceções:

FA01 (Cliente Novo): Atendente opta por cadastrar o cliente antes de finalizar (UC03).

FA02 (Estoque Insuficiente): Sistema alerta que não há saldo e impede a adição do item.
<img width="406" height="959" alt="image" src="https://github.com/user-attachments/assets/8d5320b0-318c-4104-ac5e-0b29b80f0db7" />



                     _________________________________________________________________________________                 


#UC02 — Consultar Produto
Ator(es): Atendente, Gerente.

Descrição: Pesquisar itens por nome, código ou fabricante.

Fluxo Principal: 1. Inserir termo de busca; 2. Sistema exibe lista; 3. Usuário seleciona o item.

Fluxos Alternativos / Exceções:

FE01 (Produto não encontrado): O sistema informa que o termo não gerou resultados e sugere busca por palavras-chave similares.

<img width="494" height="375" alt="image" src="https://github.com/user-attachments/assets/f7ec0f23-0b90-486f-8731-3fecdb604ad3" />



                   ___________________________________________________________________________________      

#UC03 — Cadastrar Cliente
Ator(es): Atendente.

Descrição: Registro de novos clientes no sistema.

Fluxo Principal: 1. Inserir CPF; 2. Preencher dados (Nome, Tel); 3. Salvar.

Fluxos Alternativos / Exceções:

FE01 (CPF já cadastrado): O sistema informa que o cliente já existe e oferece a opção de atualizar os dados.

<img width="436" height="405" alt="image" src="https://github.com/user-attachments/assets/c04779a0-9f56-42da-831d-de6eb12a7bed" />



                    _________________________________________________________________________________

#UC04 — Verificar Estoque (Include de UC01)
Ator(es): Sistema.

Descrição: Validação automática da quantidade física disponível.

Fluxo Principal: 1. Consultar saldo no banco; 2. Retornar "Disponível".

Fluxos Alternativos / Exceções:

FA01 (Nível Crítico): O sistema valida que há estoque, mas emite um alerta visual de que o item está abaixo do mínimo configurado.

FE01 (Saldo Zero): O sistema retorna erro e bloqueia a inserção do item na venda.


<img width="497" height="448" alt="image" src="https://github.com/user-attachments/assets/c5eaaad5-a0f7-464f-8662-ca6230230987" />



                
                    _________________________________________________________________________________  

#UC05 — Registrar Conta a Receber (Extend de UC01)
Ator(es): Sistema.

Descrição: Criação de título financeiro para vendas a prazo.

Fluxo Principal: 1. Vincular venda ao CPF do cliente; 2. Gerar parcelas/vencimento; 3. Salvar como "Aberta".

Fluxos Alternativos / Exceções:

FE01 (Cliente Inadimplente): O sistema verifica se o cliente possui contas "Atrasadas" e bloqueia a nova venda a prazo, exigindo autorização do gerente.


<img width="278" height="665" alt="image" src="https://github.com/user-attachments/assets/3ad67c78-d8f9-43d0-9851-ae1d815f2e8b" />



                    _________________________________________________________________________________

#UC06 — Manter Produto (CRUD)
Ator(es): Gerente.

Descrição: Gerenciar o catálogo de produtos.

Fluxo Principal: 1. Selecionar "Novo"; 2. Preencher campos; 3. Confirmar.

Fluxos Alternativos / Exceções:

FA01 (Exclusão): O gerente solicita excluir produto. Se houver histórico de vendas, o sistema apenas "Inativa" o produto para manter a integridade dos relatórios.


<img width="450" height="605" alt="image" src="https://github.com/user-attachments/assets/e7fbcf3c-bf72-4ede-99b8-e9ccbf0d5cb7" />


                     ________________________________________________________________________________

#UC07 — Ajustar Estoque Manualmente
Ator(es): Gerente.

Descrição: Entrada ou saída manual de itens.

Fluxo Principal: 1. Selecionar item; 2. Informar nova quantidade; 3. Justificar motivo.

Fluxos Alternativos / Exceções:

FA01 (Registro de Perda): O gerente informa que o ajuste é por validade vencida; o sistema gera um log específico para relatório de perdas.


<img width="450" height="486" alt="image" src="https://github.com/user-attachments/assets/4100c1c8-0db4-4e70-9484-923d21d9cb6a" />



                    __________________________________________________________________________________

#UC08 — Autenticar Usuário
Ator(es): Todos.

Descrição: Login no sistema.

Fluxo Principal: 1. Inserir Login; 2. Inserir Senha; 3. Acessar.

Fluxos Alternativos / Exceções:

FE01 (Senha Incorreta): O sistema nega o acesso. Após 3 tentativas, bloqueia o usuário temporariamente.


<img width="508" height="570" alt="image" src="https://github.com/user-attachments/assets/a6386049-1725-4bdc-a601-835a99632a4d" />


                     _________________________________________________________________________________

#UC09 — Gerar Relatório de Vendas
Ator(es): Gerente, Financeiro.

Descrição: Emissão de indicadores de desempenho.

Fluxo Principal: 1. Escolher período; 2. Processar; 3. Exibir.

Fluxos Alternativos / Exceções:

FA01 (Exportar PDF): O usuário opta por baixar o arquivo em vez de apenas visualizar na tela.


<img width="525" height="553" alt="image" src="https://github.com/user-attachments/assets/0976dcdd-276e-43a8-b2ab-b82fb391ff0c" />


                       ____________________________________________________________________________

#UC10 — Emitir Comprovante
Ator(es): Sistema.

Descrição: Impressão dos dados da venda.

Fluxo Principal: 1. Formatar dados; 2. Enviar para impressora.

Fluxos Alternativos / Exceções:

FE01 (Impressora Offline): O sistema notifica o erro e permite salvar o comprovante digitalmente para envio por e-mail/WhatsApp.

<img width="392" height="516" alt="image" src="https://github.com/user-attachments/assets/e3096a87-f1bf-42a2-947a-1f830e006bcb" />

