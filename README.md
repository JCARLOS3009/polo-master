## 🔍 Identificação de Itens Duplicados

O sistema identifica se dois ou mais itens são iguais extraindo o padrão de referência diretamente da descrição do serviço através da função `extractReference(desc)`.

### Regras de Extração e Chave Única:
* **Padrões Identificados:** O sistema busca no final da descrição os formatos **`CS: NÚMERO`** ou **`REF: NÚMERO`**. Se encontrados, a string gerada (ex: `CS:123456` ou `REF:7890`) torna-se a chave única do item.
* **Mecanismo de Fallback:** Caso nenhum dos padrões seja encontrado na descrição, o sistema utiliza automaticamente os **primeiros 30 caracteres da descrição** como chave de identificação.

#### Exemplos de Extração:
* `"AF_12/2021 CS: 103318"` ➡️ Chave única: `CS:103318`
* `"REF: 9072 ORSE"` ➡️ Chave única: `REF:9072`
* `"Alvenaria de bloco estrutural..."` ➡️ Chave única (Fallback): `Alvenaria de bloco estrutura`

---

## ⚙️ Fluxo de Mesclagem (`mergeAditivoIntoBase`)

Ao processar a mesclagem, cada item do aditivo passa pelas seguintes etapas de validação hierárquica:

1. **Validação de Categoria:** Localiza a categoria na planilha base usando o número correspondente (`catNum`). Se não existir, uma nova categoria é criada.
2. **Validação de Subcategoria:** Dentro da categoria localizada, busca a subcategoria pelo código (ex: `subCode: "1.1"`). Se não existir, cria uma nova subcategoria.
3. **Busca por Duplicidade:** Restrito **apenas** ao escopo dessa subcategoria específica, o sistema procura por um item existente que possua a mesma referência (`ref`) do item do aditivo.

### Resultados da Busca:
* **Se encontrar o item:** O sistema mantém o código original da base (coluna `ITEM`), soma a quantidade do aditivo à quantidade já existente e recalcula o valor total (`quantidade * preçoUnitario`).
* **Se NÃO encontrar o item:** O item é adicionado como novo, gerando um código sequencial inédito para aquela subcategoria (ex: `1.1.74`).

---

## ⚠️ Comportamento de Itens Iguais em Categorias Diferentes

> 💡 **Regra de Escopo Crucial:** A busca por duplicidade é restrita estritamente ao escopo da subcategoria corrente. O sistema **não varre** outras subcategorias ou categorias procurando pela mesma referência.

Se o mesmo item (com a mesma referência) aparecer no aditivo apontando para uma categoria ou subcategoria diferente da base, ele **será tratado como um item novo**. 

* **Exemplo Prático:** Se a base possui a referência `CS:12345` na subcategoria `1.2`, e o aditivo traz o mesmo `CS:12345` apontando para a subcategoria `1.3`, o sistema criará um novo item na subcategoria `1.3`. O resultado final terá dois itens distintos com a mesma referência no sistema, cada um com sua própria quantidade e código correspondente ao seu escopo.

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
