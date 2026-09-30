# Projeto ERP — Festa & Cia Locações
Sistema de Locação de Produtos para Festas
Universidade Cidade de São Paulo — UNICID | Modelagem de Banco de Dados
Orientador: Prof. Clóvis Ferraro | São Paulo, 2026

## 1. Identificação da Equipe
| Nome | RGM |
|---|---|
| Ana Carolina Santana dos Santos | 47505249 |
| Cesar Augusto Vivoda Cruz | 49455273 |
| Danilo Gomes | 04933256-2 |
| Érika Gabriela Bueno da Silva | 48836061 |
| Grazielle Pinheiro Barreto | 04940761-9 |
| João Pedro Moreira Dias Pereira | 49067095 |
| Julia Costa de Jesus | 46063978 |
| Lethicia Gomes de Souza | 49700995 |
| Leticia Alarcon Gomes de Lima | 04989967-8 |
| Lucas Moreira Lima | 04928488-6 |

## 2. Caracterização da Empresa
**Nome:** Festa & Cia Locações
**Segmento:** Locação de produtos e equipamentos para festas e eventos
**O que oferece:** Mesas, cadeiras, toalhas, louças, decorações temáticas, tendas, iluminação, pula-pulas, camas elásticas e brinquedos para aniversários, casamentos e eventos corporativos.
**Clientes:** Pessoas Físicas (aniversários, casamentos) e Pessoas Jurídicas (buffets, decoradores, cerimonialistas).
**Setores:** Comercial/Atendimento, Estoque/Logística, Financeiro.

## 3. Justificativa da Escolha
Escolhida por possuir processos bem definidos e ideais para modelagem relacional: cadastro de clientes, controle de estoque por data, geração de contratos, logística de retirada/devolução e gestão financeira de multas. Atualmente o controle é manual (WhatsApp, cadernos, planilhas), gerando necessidade clara de integração via ERP.

## 4. Problemas Identificados
1. Controle de reservas via WhatsApp/caderno — conflito de datas
2. Estoque manual sem baixa automática — divergência de saldo
3. Cadastro de clientes pulverizado — duplicidade e perda de histórico
4. Falta de comunicação entre Comercial, Estoque e Financeiro
5. Ausência de registro padronizado de avarias e multas
6. Contratos e pagamentos em papel — dificuldade de acompanhamento
7. Ausência de relatórios consolidados
8. Falta de controle de privilégios de acesso

## 5. Processos de Negócio
1. **Módulo 0 - Administração:** criação de usuários e perfis
2. **Módulo 1 - Cadastro de Cliente:** validação CPF/CNPJ e e-mail únicos
3. **Módulo 2 - Login do Cliente:** autenticação com 2FA
4. **Módulo 3 - Catálogo, Cotação e Locação:** cotação com validade 48h, conversão com sinal de 50%
5. **Módulo 4 - Produtos e Estoque:** controle de status (Disponível, Locado, Em Manutenção, Inutilizado)
6. **Módulo 5 - Contrato e Financeiro:** contrato digital, Pix/Boleto/Cartão, NF após quitação
7. **Módulo 6 - Devolução, Avarias e Multas:** vistoria com laudo fotográfico
8. **Módulo 7 - Relatórios e Dashboard:** faturamento, curva ABC, inadimplência

## 6. Requisitos Funcionais (resumo)
**Principais:** RF Cadastro com CPF/e-mail únicos, RF Múltiplos endereços, RF Catálogo com disponibilidade por data, RF Cotação com validade 48h, RF Conversão em locação após sinal 50%, RF Baixa lógica de estoque, RF Contrato PDF automático, RF Multa por atraso >24h (1 diária + 10%/dia), RF Classificação avaria (Leve 20%, Média 50%, Grave 100%).

*Lista completa no documento original.*

## 7. Requisitos Não Funcionais
- RNF01 Desempenho: consulta de disponibilidade < 3s
- RNF02 Segurança: senhas criptografadas, 2FA, logs de auditoria
- RNF03 Usabilidade: interface responsiva mobile/web
- RNF04 LGPD: termo de consentimento com data/hora
- RNF05 Disponibilidade: 99% em horário comercial

## 8. Regras de Negócio
- RN01: Cotação válida por 48h. Sem sinal, reserva é liberada.
- RN02: Sinal de 50% obrigatório para converter cotação em locação.
- RN03: CPF/CNPJ e e-mail devem ser únicos.
- RN04: Cliente menor de 18 anos não pode cadastrar.
- RN05: Cancelamento: 100% reembolso >48h, 50% entre 24-47h, 0% <24h.
- RN06: Atraso >24h gera 1 diária + 10% multa/dia. Após 7 dias, cobrança de reposição integral.
- RN07: Uma cotação gera no máximo uma locação.

## 9. Restrições e Políticas Organizacionais
- Acesso por perfis: Admin Master, Comercial/Vendas, Estoque/Logística
- Bloqueio após 3 tentativas de login
- Sessão expira após 15 min de inatividade
- Assinatura digital obrigatória no contrato

## 10. Fluxogramas
![Fluxo 00](Flowchart%20(00).jpg)
![Fluxo 01](Flowchart%20(01).jpg)
![Fluxo 02](Flowchart%20(02).jpg)
![Fluxo 03](Flowchart%20(03).jpg)
![Fluxo 04](Flowchart%20(04).jpg)
![Fluxo 05](Flowchart%20(05).jpeg)
![Fluxo 06](Flowchart%20(06).jpeg)
![Fluxo 07](Flowchart%20(07).jpeg)

## 11. Entidades
PESSOA, CLIENTE, FUNCIONARIO, ENDERECO, PESSOA_ENDERECO, CATEGORIA, PRODUTO, COTACAO, ITEM_COTACAO, LOCACAO, PAGAMENTO, AVARIA, MULTA

## 12. Atributos
| Entidade | Principais Atributos |
|---|---|
| PESSOA | id_pessoa (PK), nome, cpf_cnpj (UK), email (UK), telefone, tipo_pf_pj |
| CLIENTE | id_pessoa (PK/FK), status (Ativo/Locado/Bloqueado), aceite_lgpd_data |
| PRODUTO | id_produto (PK), sku (UK), nome, qtd_estoque, valor_diaria, status |
| COTACAO | id_cotacao (PK), data_emissao, validade_48h, valor_total, status |
| LOCACAO | id_locacao (PK), data_festa, data_retirada, data_devolucao, valor_total |

## 13. Relacionamentos
- PESSOA – CLIENTE: especialização para solicitar cotações
- PESSOA – FUNCIONARIO: especialização para operar o sistema
- PESSOA – PESSOA_ENDERECO – ENDERECO: N:N de endereços
- CLIENTE — COTACAO: solicita orçamentos
- CATEGORIA — PRODUTO: agrupa produtos
- COTACAO — ITEM_COTACAO — PRODUTO: especifica produtos e quantidades
- COTACAO — LOCACAO: cotação aprovada origina locação
- LOCACAO — PAGAMENTO: gera sinal e saldo
- LOCACAO — AVARIA: pode originar laudo
- AVARIA — MULTA: gera cobrança
- FUNCIONARIO — AVARIA: assina vistoria

## 14. Cardinalidades
PESSOA (1,1) — (0,1) CLIENTE | CLIENTE (1,1) — (0,n) COTACAO | COTACAO (1,1) — (1,n) ITEM_COTACAO | COTACAO (0,1) — (1,1) LOCACAO | LOCACAO (1,1) — (1,n) PAGAMENTO

## 15. Dicionário de Dados Conceitual
Ver planilha em `/docs/dicionario-dados.xlsx`

## 16. DER
### Conceitual
![DER Conceitual](DIAGRAMA.JPG)


## 17. Justificativas Técnicas
- Generalização PESSOA evita duplicar dados entre cliente e funcionário
- PESSOA_ENDERECO resolve N:N e permite compartilhar endereço
- ITEM_COTACAO guarda quantidade e valor histórico, pois o preço pode mudar
- LOCACAO separada de COTACAO para manter histórico de orçamentos não convertidos

## 18. Conclusão
O projeto centraliza em um banco relacional único as informações hoje dispersas, eliminando conflitos de reserva, divergências de estoque e prejuízos com avarias, servindo como núcleo para um futuro ERP modular.
