#  Módulo de Checkout & Carrinho de Compras — Testes de Software

Este repositório contém a suíte completa de **testes manuais e automatizados** desenvolvida para homologação da lógica de cálculo e regras de negócio do módulo de **Checkout / Carrinho de Compras** de uma plataforma de e-commerce construída em **Node.js**.

O projeto contempla a identificação de falhas na versão inicial (`1.0.0 bugged`), documentação do **Guia de Ordem de Teste (GOT)**, implementação de suíte automatizada com **Jest** e refatoração do código até a cobertura completa dos cenários.

---

##  Visão Geral do Projeto

* **Sistema:** E-Commerce Node.js  
* **Módulo:** Checkout — Carrinho de Compras  
* **Ambiente de Testes:** Node.js v18 / Jest  
* **Responsável:** Giovana De Moura & Equipe de QA  
* **Data:** 21/08/2026  
* **Status:**  **Aprovado em Homologação** (6/6 cenários com status *PASS*)

---

##  Regras de Negócio Validadas

A aplicação foi submetida a verificações rigorosas baseadas na matriz de regras de negócio estabelecida para a release:

1. **Cálculo de Subtotal**
   * Multiplicação do preço unitário pela quantidade de cada item inserido no carrinho.
2. **Validação e Tratamento de Exceções**
   * Lançamento de erro com a mensagem exata `"Carrinho inválido"` caso:
     * O carrinho esteja vazio (`[]`).
     * Algum item possua quantidade menor ou igual a zero (<= 0).
     * Algum item possua preço unitário negativo (< 0).
3. **Regra de Cupom de Desconto**
   * Ao aplicar o cupom `"PROMO10"`, a aplicação deve conceder **10% de desconto** sobre o valor total do subtotal.
4. **Política de Frete**
   * **Frete Grátis (R$ 0,00):** Para compras com subtotal maior ou igual a R$ 100,00.
   * **Frete Fixo (R$ 15,00):** Para compras com subtotal menor que R$ 100,00.
5. **Precisão Monetária**
   * Arredondamento e formatação do valor final do pedido para exatamente **2 casas decimais**.

---

## 📋 Guia de Ordem de Teste (GOT) & Matriz de Rastreabilidade

| ID | Cenário / Regra | Dados de Entrada | Resultado Esperado | Resultado Inicial | Status Inicial | Status Final |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: |
| **CT-01** | Frete Grátis na borda | Item: R$ 100,00 \| Qtd: 1 \| Cupom: `null` | Total: `100.00` | Total: `115.00` | ❌ Falhou | ✅ Passou |
| **CT-02** | Desconto com Cupom 10% | Item: R$ 50,00 \| Qtd: 1 \| Cupom: `"PROMO10"` | Total: `60.00` *(50 - 5 + 15)* | Total: `55.00` *(50 - 10 + 15)* | ❌ Falhou | ✅ Passou |
| **CT-03** | Validação de Quantidade Negativa | Item: R$ 100,00 \| Qtd: -2 \| Cupom: `null` | Erro: `"Carrinho inválido"` | Erro: `"Carrinho inválido"` | ✅ Passou | ✅ Passou |
| **CT-04** | Arredondamento Monetário | Item: R$ 33,333 \| Qtd: 1 \| Cupom: `null` | Total: `48.33` *(33.33 + 15)* | Total: `48.33333333333336` | ❌ Falhou | ✅ Passou |
| **CT-05** | Validação de Carrinho Vazio | Itens: `[]` \| Cupom: `null` | Erro: `"Carrinho inválido"` | Erro: `"Carrinho inválido"` | ✅ Passou | ✅ Passou |
| **CT-06** | Cobrança de Frete Pago | Item: R$ 80,00 \| Qtd: 1 \| Cupom: `null` | Total: `95.00` *(80 + 15)* | Total: `95.00` | ✅ Passou | ✅ Passou |

---

## 📁 Estrutura do Repositório

```text
carrinho_testes/
├── carrinho.js          # Módulo principal com a regra de negócio do carrinho
├── carrinho.test.js     # Suíte de testes automatizados com Jest
├── index.js             # Script para execução e validação dos testes manuais
└── package.json         # Manifesto do projeto e scripts de execução de testes
