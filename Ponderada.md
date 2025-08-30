# Similaridade de Cosseno aplicada à acessibilidade (relatório de aluno)

A ponderada proposta pela professora Maria Cristina consiste em verificar quais frases consideradas “problemáticas” (no enunciado) estão **mais alinhadas** ou **mais distantes** de um pequeno conjunto de **boas práticas** (também do enunciado) de acessibilidade usando **vetorização de palavras** e **similaridade de cosseno**.

---
# 1. Monte o vocabulário comum

*Leia todas as frases (conteúdos e boas práticas). Elabore uma lista única de palavras (sem acentos, minúsculas, sem repetições).*

Eu li as cinco frases (3 problemáticas e 2 de boas práticas), normalizei tudo para **minúsculas**, **sem acentos** e **sem pontuação**, e depois separei por espaços. Não usei stemming/lematização (logo, `video` e `videos` são palavras diferentes). A partir disso, gerei **um único vocabulário** sem repetições.

**Vocabulário obtido (38 palavras):**
`a, acessar, adicione, com, conseguem, conteudo, de, deficiencia, descricao, e, em, esse, garanta, imagem, imagens, interprete, legenda, leitores, leitura, libras, nao, nem, o, os, ou, para, plataforma, por, possui, suporte, tela, tem, todos, traducao, usuarios, video, videos, visual`

---

# 2. Construa a matriz de vetores

*Para cada frase (3 conteúdos e 2 boas práticas), conte quantas vezes cada palavra do vocabulário aparece.*

Abaixo está a **matriz TF** (frequência simples). Cada célula é o número de ocorrências da palavra (linha) em cada frase (coluna). As cinco colunas são: **F6**, **F7**, **F8** (frases problemáticas), e **BP5**, **BP6** (boas práticas).

| Palavra     | F6 | F7 | F8 | BP5 | BP6 |
| ----------- | -: | -: | -: | --: | --: |
| a           |  0 |  0 |  1 |   0 |   0 |
| acessar     |  0 |  1 |  0 |   0 |   0 |
| adicione    |  0 |  0 |  0 |   1 |   0 |
| com         |  0 |  1 |  0 |   0 |   0 |
| conseguem   |  0 |  1 |  0 |   0 |   0 |
| conteudo    |  0 |  1 |  0 |   0 |   0 |
| de          |  1 |  0 |  1 |   1 |   2 |
| deficiencia |  0 |  1 |  0 |   0 |   0 |
| descricao   |  0 |  0 |  1 |   0 |   1 |
| e           |  0 |  0 |  0 |   1 |   1 |
| em          |  0 |  0 |  1 |   0 |   0 |
| esse        |  1 |  0 |  0 |   0 |   0 |
| garanta     |  0 |  0 |  0 |   0 |   1 |
| imagem      |  0 |  0 |  1 |   0 |   0 |
| imagens     |  0 |  0 |  0 |   0 |   1 |
| interprete  |  1 |  0 |  0 |   1 |   0 |
| legenda     |  1 |  0 |  0 |   1 |   0 |
| leitores    |  0 |  0 |  0 |   0 |   1 |
| leitura     |  0 |  0 |  0 |   0 |   1 |
| libras      |  1 |  0 |  1 |   1 |   0 |
| nao         |  1 |  1 |  1 |   0 |   0 |
| nem         |  1 |  0 |  0 |   0 |   0 |
| o           |  0 |  1 |  0 |   0 |   0 |
| os          |  0 |  0 |  0 |   1 |   0 |
| ou          |  0 |  0 |  1 |   0 |   0 |
| para        |  0 |  0 |  1 |   1 |   0 |
| plataforma  |  0 |  0 |  1 |   0 |   0 |
| por         |  0 |  0 |  0 |   0 |   1 |
| possui      |  1 |  0 |  0 |   0 |   0 |
| suporte     |  0 |  0 |  1 |   0 |   0 |
| tela        |  0 |  0 |  0 |   0 |   1 |
| tem         |  0 |  0 |  1 |   0 |   0 |
| todos       |  0 |  0 |  0 |   1 |   0 |
| traducao    |  0 |  0 |  1 |   0 |   0 |
| usuarios    |  0 |  1 |  0 |   0 |   0 |
| video       |  1 |  0 |  0 |   0 |   0 |
| videos      |  0 |  0 |  0 |   1 |   0 |
| visual      |  0 |  1 |  0 |   0 |   0 |

Para referência, os **comprimentos (normas)** dos vetores (raiz da soma dos quadrados das frequências) são:

* ||F6|| = √9 = **3,0000**
* ||F7|| = √9 = **3,0000**
* ||F8|| = √13 ≈ **3,6056**
* ||BP5|| = √10 ≈ **3,1623**
* ||BP6|| = √12 ≈ **3,4641**

---

# 3. Calcule a similaridade de cosseno

*Faça 6 cálculos (3 frases × 2 boas práticas).*

A **similaridade de cosseno** entre dois vetores $A$ e $B$ é:

$$
\cos(A,B)=\frac{\sum_i A_i B_i}{\sqrt{\sum_i A_i^2}\cdot\sqrt{\sum_i B_i^2}}
$$

Na prática, o numerador (produto escalar) soma apenas as **palavras em comum**, multiplicando as frequências correspondentes.

A seguir, descrevo um cálculo por extenso e depois listo os seis resultados.

**Exemplo detalhado — F6 × BP5**
Palavras em comum: `de(1×1)`, `interprete(1×1)`, `legenda(1×1)`, `libras(1×1)`.
Produto escalar = 1+1+1+1 = **4**.
||F6|| = 3,0000 ; ||BP5|| = 3,1623.
$\cos(F6,BP5) = 4 / (3,0000 × 3,1623) = \mathbf{0,4216}$.

**Resultados dos 6 cossenos:**

| Par      | Produto escalar | \|\|Frase\|\| | \|\|Boa\|\| | Similaridade |
| -------- | --------------: | ------------: | ----------: | -----------: |
| F6 × BP5 |               4 |        3,0000 |      3,1623 |   **0,4216** |
| F6 × BP6 |               2 |        3,0000 |      3,4641 |   **0,1925** |
| F7 × BP5 |               0 |        3,0000 |      3,1623 |   **0,0000** |
| F7 × BP6 |               0 |        3,0000 |      3,4641 |   **0,0000** |
| F8 × BP5 |               3 |        3,6056 |      3,1623 |   **0,2631** |
| F8 × BP6 |               3 |        3,6056 |      3,4641 |   **0,2402** |

---

# 4. Interprete os resultados

*Qual frase está mais distante das boas práticas? Qual está mais próxima?*

Falando de forma simples: **quanto maior** a similaridade, **mais próxima** a frase está da boa prática (maior sobreposição de termos); **quanto menor** (próximo de zero), **mais distante** ela está.

* **Mais próxima:** a frase **F6** é a que melhor se aproxima das boas práticas, especialmente de **BP5** (cosseno **0,4216**). Isso acontece porque o vocabulário de F6 menciona exatamente os mesmos pilares de BP5 — *legenda*, *intérprete* e *Libras* — resultando em várias palavras em comum no produto escalar.
* **Mais distante:** a frase **F7** é a mais distante, pois sua similaridade é **0,0000** tanto com **BP5** quanto com **BP6**. Ela fala de *deficiência visual*, *acessar* e *conteúdo*, mas **não compartilha termos** com as boas práticas fornecidas (que focam em *legendas*, *intérprete/Janela de Libras*, *descrição de imagens* e *leitura por leitores de tela*).

**Resumo interpretativo:**

* **Prioridade de correção:** comece por **F7**, incorporando termos e, principalmente, **ações** das boas práticas (ex.: “garantir descrição de imagens” e “leitura por leitores de tela”) para aumentar a aderência.
* **F8** tem alguma aderência às duas boas práticas (por *libras*, *descricao*, *para*, *de*), ficando no meio do caminho.
* **F6** já está razoavelmente alinhada com **BP5**, porque toca diretamente nos recursos de acessibilidade para pessoas surdas/usuárias de Libras (legenda e intérprete).
