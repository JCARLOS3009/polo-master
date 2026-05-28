## 🔍 Identificação de Itens Duplicados

O sistema identifica se dois ou mais itens são iguais extraindo o padrão de referência diretamente da descrição do serviço. A busca é feita com base nos seguintes formatos:
* **`CS: NÚMERO`**
* **`REF: NÚMERO`**

### Exemplos de Extração:
* `"AF_12/2021 CS: 103318"` ➡️ Referência identificada: `CS:103318`
* `"REF: 9072 ORSE"` ➡️ Referência identificada: `REF:9072`

> ⚠️ **Regra de Negócio:** Itens que possuem a **mesma referência** e pertencem à **mesma subcategoria** são considerados o mesmo serviço. Nesse cenário, as quantidades são somadas e o código da coluna `ITEM` permanece idêntico ao da planilha base.

---

## 🧪 Testes Unitários Implementados

Os testes garantem a integridade das regras de agrupamento e criação de novos itens. Os cenários validados são:

* **Cenário 1 (Item existente na base):** Garante que o sistema identifica a referência existente e apenas soma as quantidades.
* **Cenário 2 (Item novo):** Valida se o sistema gera um novo código corretamente quando a referência não é encontrada.
* **Cenário 3 (Múltiplos itens iguais):** Verifica se a soma acumulada funciona perfeitamente ao processar vários itens com a mesma referência em lote.
* **Cenário 4 (Separação de padrões):** Garante que referências do tipo `CS:` e `REF:` são tratadas de forma totalmente isolada, mesmo que possuam o mesmo número.

### Como Executar os Testes
1. Abra a página do sistema no navegador.
2. Clique no botão **“🧪 Executar Testes Unitários”**.
3. Os resultados detalhados serão exibidos diretamente no **log da página** e no **console do navegador** (F12).
