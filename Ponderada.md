# Similaridade de Cosseno aplicada à acessibilidade (relatório de aluno)

A atividade consiste em verificar, com **vetorização de palavras** e **similaridade de cosseno**, quais frases “problemáticas” estão **mais alinhadas** ou **mais distantes** de um pequeno conjunto de **boas práticas** de acessibilidade.

---

## 1) Dados usados

**Frases problemáticas**

* F6 — “Esse vídeo não possui legenda nem intérprete de Libras.”
* F7 — “Usuários com deficiência visual não conseguem acessar o conteúdo.”
* F8 — “A plataforma não tem suporte para tradução em Libras ou descrição de imagem.”

**Boas práticas**

* BP5 — “Adicione legenda e intérprete de Libras para todos os vídeos.”
* BP6 — “Garanta descrição de imagens e leitura por leitores de tela.”

---

## 2) Como eu pré-processei o texto

Para conseguir comparar frases curtas de modo matemático, transformei cada frase em uma lista de palavras “limpas”. Fiz o seguinte:

1. **Minúsculas** (ex.: “Vídeo” → “vídeo”).
2. **Remoção de acentos** (ex.: “vídeo” → “video”; “Libras” fica “libras” mas sem acentos).
3. **Remoção de pontuação** (vírgulas, pontos etc.).
4. **Separação por espaços** (tokenização).
5. **Sem** stemming/lematização: eu **não** reduzi palavras à raiz. Assim, “video” é diferente de “videos”.

Com isso, as frases ficaram assim (exemplos, já sem acentos e em minúsculas):

* **F6** → `esse video nao possui legenda nem interprete de libras`
* **F7** → `usuarios com deficiencia visual nao conseguem acessar o conteudo`
* **F8** → `a plataforma nao tem suporte para traducao em libras ou descricao de imagem`
* **BP5** → `adicione legenda e interprete de libras para todos os videos`
* **BP6** → `garanta descricao de imagens e leitura por leitores de tela`

Em seguida, montei um **vocabulário comum**: é a lista única de todas as palavras que apareceram em qualquer frase (sem repetir). Ex.:
`a, acessar, adicione, com, conseguem, conteudo, de, deficiencia, descricao, e, em, esse, garanta, imagem, imagens, interprete, legenda, leitores, leitura, libras, nao, nem, o, os, ou, para, plataforma, por, possui, suporte, tela, tem, todos, traducao, usuarios, video, videos, visual`

---

## 3) Como eu transformei as frases em vetores

Cada frase virou um **vetor de frequências (TF)**: para **cada palavra do vocabulário**, eu conto **quantas vezes** ela aparece na frase.

Exemplo prático para duas frases, apenas nas palavras relevantes:

* **F6** tem: `legenda:1`, `interprete:1`, `libras:1`, `de:1`, `video:1`, `nao:1`, `nem:1`, `possui:1`, `esse:1`.
* **BP5** tem: `legenda:1`, `interprete:1`, `libras:1`, `de:1`, `para:1`, `todos:1`, `os:1`, `videos:1`, `adicione:1`, `e:1`.

O vetor é, portanto, uma sequência de números (0, 1, 2, …) alinhada ao vocabulário.
Para cálculo manual, como as contagens são pequenas, fica simples.

Também calculei o **comprimento** (norma Euclidiana) de cada vetor, pois a fórmula do cosseno precisa disso. Como aqui as frequências são praticamente todas 0 ou 1 (exceto “de” em BP6, que aparece 2 vezes), as normas ficaram:

* ||F6|| = √(9) = **3.0000**
* ||F7|| = √(9) = **3.0000**
* ||F8|| = √(13) ≈ **3.6056**
* ||BP5|| = √(10) ≈ **3.1623**
* ||BP6|| = √(12) ≈ **3.4641**  *(aqui conta “de” = 2, então entra 2² = 4 na soma)*

---

## 4) Como eu calculei a similaridade de cosseno

A **similaridade de cosseno** entre dois vetores $A$ e $B$ é:

$$
\text{cos}(A,B)=\frac{A\cdot B}{\|A\|\,\|B\|}
$$

* O **produto escalar** $A\cdot B$ soma **apenas as palavras em comum**, multiplicando as contagens correspondentes.
* Depois, divido pelo produto dos comprimentos (||A|| × ||B||).

Fiz **seis cálculos** (3 frases × 2 boas práticas), sempre mostrando:
palavras em comum → produto escalar → divisão pelas normas → resultado.

### 4.1) F6 × BP5

* **Comuns:** `de(1×1)`, `interprete(1×1)`, `legenda(1×1)`, `libras(1×1)`
* **Produto escalar:** 1 + 1 + 1 + 1 = **4**
* **Normas:** ||F6|| = 3.0000 ; ||BP5|| = 3.1623
* **Similaridade:** $4 / (3.0000 × 3.1623) = \mathbf{0.4216}$

### 4.2) F6 × BP6

* **Comuns:** `de(1×2)`
* **Produto escalar:** **2**
* **Normas:** 3.0000 e 3.4641
* **Similaridade:** $2 / (3.0000 × 3.4641) = \mathbf{0.1925}$

### 4.3) F7 × BP5

* **Comuns:** *(nenhuma)*
* **Produto escalar:** **0**
* **Normas:** 3.0000 e 3.1623
* **Similaridade:** **0.0000**

### 4.4) F7 × BP6

* **Comuns:** *(nenhuma)*
* **Produto escalar:** **0**
* **Normas:** 3.0000 e 3.4641
* **Similaridade:** **0.0000**

### 4.5) F8 × BP5

* **Comuns:** `de(1×1)`, `libras(1×1)`, `para(1×1)`
* **Produto escalar:** **3**
* **Normas:** 3.6056 e 3.1623
* **Similaridade:** $3 / (3.6056 × 3.1623) = \mathbf{0.2631}$

### 4.6) F8 × BP6

* **Comuns:** `de(1×2)`, `descricao(1×1)`
* **Produto escalar:** **3**
* **Normas:** 3.6056 e 3.4641
* **Similaridade:** $3 / (3.6056 × 3.4641) = \mathbf{0.2402}$

Para facilitar a leitura, aqui está o **quadro-resumo** com os valores finais (apenas números):

| Frase |    BP5 |    BP6 |
| ----- | -----: | -----: |
| F6    | 0.4216 | 0.1925 |
| F7    | 0.0000 | 0.0000 |
| F8    | 0.2631 | 0.2402 |

---

## 5) O que esses números significam (interpretação)

Quando a similaridade é **mais alta**, a frase problemática está **mais próxima** da linguagem das boas práticas. Quando é **baixa** (ou zero), está **mais distante**.

* **Mais próxima das boas práticas:** **F6**, especialmente em relação à **BP5** (0.4216).
  Isso acontece porque F6 e BP5 compartilham diretamente as palavras-chave **“legenda”, “intérprete”, “Libras”** e **“de”**. Ou seja, F6, embora relate um problema (“não possui”), fala **exatamente dos recursos** apontados como recomendação.

* **Mais distante das boas práticas:** **F7**, tanto em relação a **BP5** quanto a **BP6** (**0.0000** com ambas).
  F7 fala de **deficiência visual**, **acesso** e **conteúdo**, enquanto as boas práticas citadas tratam de **legendas e intérprete de Libras** (BP5) e de **descrição de imagens** e **leitura por leitores de tela** (BP6). Como o vocabulário **não se cruza**, o cosseno zera.

* **F8** fica em posição intermediária, com sobreposição parcial a ambas:
  com **BP5** por “**libras**”, “**para**”, “**de**”; com **BP6** por “**descricao**” e “**de**”.

Se eu converter “similaridade” em uma ideia de **distância do cosseno** (1 − similaridade), F7 seria o caso **mais distante** de ambos os guias; e F6, o **mais próximo** de BP5.

---

## 6) Conclusão (entregável “c”)

* **Qual frase foi a pior e por quê?**
  **F7** é a pior em relação às boas práticas dadas, pois sua similaridade de cosseno com **BP5** e **BP6** é **zero**. Isso indica **nenhum alinhamento lexical** com recomendações como **legendas**, **intérprete de Libras**, **descrição de imagens** e **leitores de tela**.
  Uma reescrita de F7 deveria **introduzir explicitamente** esses recursos para aproximar o texto das recomendações (ex.: “garantir descrição de imagens e suporte completo a leitores de tela”).

* **Qual está mais próxima?**
  **F6** está mais próxima de **BP5**, pois usa **o mesmo vocabulário central** das recomendações (legendas, intérprete, Libras) — apesar de indicar ausência dos recursos, trata exatamente do que deve ser provido.

---

## 7) O que eu entregaria (entregáveis “a” e “b”)

1. **Planilha com vocabulário e vetores (TF)**: uma aba com o **vocabulário comum** nas linhas e as **frases** nas colunas, contendo as **contagens** (0/1/2).
2. **Cálculos das similaridades**: outra aba (ou um bloco no próprio arquivo) com, para cada par **(Frase, BP)**, as **palavras em comum**, o **produto escalar**, as **normas** e a **similaridade** já numérica (os seis resultados mostrados acima).

> Observação: este relatório já traz, por extenso, todo o **passo a passo** e os **valores numéricos** necessários para conferir a planilha. Se você quiser, eu também disponibilizo a planilha em Excel e o CSV de resultados.
