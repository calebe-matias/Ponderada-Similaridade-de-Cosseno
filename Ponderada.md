# Similaridade de Cosseno Aplicada à Acessibilidade Digital

*(Relatório de aluno — passo a passo com todos os cálculos)*

## Dados de entrada

**Frases Problemáticas**

* **F6:** “Esse vídeo não possui legenda nem intérprete de Libras.”
* **F7:** “Usuários com deficiência visual não conseguem acessar o conteúdo.”
* **F8:** “A plataforma não tem suporte para tradução em Libras ou descrição de imagem.”

**Boas Práticas**

* **BP5:** “Adicione legenda e intérprete de Libras para todos os vídeos.”
* **BP6:** “Garanta descrição de imagens e leitura por leitores de tela.”

---

## 1) Pré-processamento e vocabulário comum

**Regras aplicadas:**

1. tudo minúsculo; 2) remoção de acentos; 3) remoção de pontuação; 4) tokenização por espaço; 5) **sem** stemming/lemmatização (logo, *video* ≠ *videos*).

**Frases normalizadas (exemplos):**

* F6 → `esse video nao possui legenda nem interprete de libras`
* F7 → `usuarios com deficiencia visual nao conseguem acessar o conteudo`
* F8 → `a plataforma nao tem suporte para traducao em libras ou descricao de imagem`
* BP5 → `adicione legenda e interprete de libras para todos os videos`
* BP6 → `garanta descricao de imagens e leitura por leitores de tela`

**Vocabulário comum (único, sem acentos, minúsculas):**
`a, acessar, adicione, com, conseguem, conteudo, de, deficiencia, descricao, e, em, esse, garanta, imagem, imagens, interprete, legenda, leitores, leitura, libras, nao, nem, o, os, ou, para, plataforma, por, possui, suporte, tela, tem, todos, traducao, usuarios, video, videos, visual`

---

## 2) Matriz de vetores (TF — frequência simples)

Cada célula indica **quantas vezes** a palavra (linha) aparece na frase (coluna).
*(Apenas palavras e números nas tabelas, sem frases longas.)*

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

**Normas (comprimentos) dos vetores**

* ||F6|| = √(9) = **3.0000**
* ||F7|| = √(9) = **3.0000**
* ||F8|| = √(13) ≈ **3.6056**
* ||BP5|| = √(10) ≈ **3.1623**
* ||BP6|| = √(12) ≈ **3.4641**

---

## 3) Similaridade de cosseno (6 cálculos)

**Fórmula:**

$$
\text{cos}(A,B)=\frac{\sum_i A_i B_i}{\sqrt{\sum_i A_i^2}\cdot\sqrt{\sum_i B_i^2}}
$$

> **Observação:** O produto escalar envolve **apenas as palavras em comum**, multiplicando as frequências correspondentes e somando.

### 3.1) F6 × BP5

* Palavras em comum: `de(1×1)`, `interprete(1×1)`, `legenda(1×1)`, `libras(1×1)`
* **Produto escalar:** 1+1+1+1 = **4**
* ||F6|| = 3.0000 ; ||BP5|| = 3.1623
* **cos(F6,BP5) = 4 / (3.0000 × 3.1623) = 0.4216**

### 3.2) F6 × BP6

* Palavras em comum: `de(1×2)`
* **Produto escalar:** 2
* ||F6|| = 3.0000 ; ||BP6|| = 3.4641
* **cos(F6,BP6) = 2 / (3.0000 × 3.4641) = 0.1925**

### 3.3) F7 × BP5

* Palavras em comum: *(nenhuma)*
* **Produto escalar:** 0
* ||F7|| = 3.0000 ; ||BP5|| = 3.1623
* **cos(F7,BP5) = 0.0000**

### 3.4) F7 × BP6

* Palavras em comum: *(nenhuma)*
* **Produto escalar:** 0
* ||F7|| = 3.0000 ; ||BP6|| = 3.4641
* **cos(F7,BP6) = 0.0000**

### 3.5) F8 × BP5

* Palavras em comum: `de(1×1)`, `libras(1×1)`, `para(1×1)`
* **Produto escalar:** 3
* ||F8|| = 3.6056 ; ||BP5|| = 3.1623
* **cos(F8,BP5) = 3 / (3.6056 × 3.1623) = 0.2631**

### 3.6) F8 × BP6

* Palavras em comum: `de(1×2)`, `descricao(1×1)`
* **Produto escalar:** 3
* ||F8|| = 3.6056 ; ||BP6|| = 3.4641
* **cos(F8,BP6) = 3 / (3.6056 × 3.4641) = 0.2402**

**Resumo numérico (cos(A,B)):**

| Frase |    BP5 |    BP6 |
| ----- | -----: | -----: |
| F6    | 0.4216 | 0.1925 |
| F7    | 0.0000 | 0.0000 |
| F8    | 0.2631 | 0.2402 |

*(Opcional: distância do cosseno = 1 − similaridade.)*

---

## 4) Interpretação dos resultados

* **Mais próxima das boas práticas:** **F6 ↔ BP5** (0.4216).
  *Motivo:* léxico fortemente alinhado com *“legenda”, “intérprete”, “Libras”* — exatamente os itens pedidos em BP5.
* **Mais distante das boas práticas:** **F7** (0.0000 com BP5 e BP6).
  *Motivo:* vocabulário não se cruza com as boas práticas fornecidas (fala de “deficiência visual”, “acessar” e “conteúdo”, enquanto BP5/BP6 focam em “legendas”, “Libras”, “descrição de imagens” e “leitura por leitores de tela”).
* **F8** fica no meio do caminho, com sobreposição a ambos os guias:

  * com **BP5** por “libras” e “para”;
  * com **BP6** por “descricao” e “de”.

**Resposta direta (pior frase e por quê):**

> **F7** é a pior em relação às boas práticas fornecidas porque sua similaridade de cosseno é **zero** com **BP5** e **BP6**, indicando **nenhuma sobreposição lexical** com as recomendações de acessibilidade (legendas, Libras, descrição de imagens e leitura por leitores de tela).

---

## 5) Conclusões e recomendações

* **Prioridade de ajuste:** comece por **F7**, incorporando elementos das boas práticas, por exemplo:
  “*Garanta descrição de imagens e leitura por leitores de tela; adapte o conteúdo para acessibilidade de pessoas com deficiência visual.*”
* **Ajustes menores em F8:** incluir também “legendas” e “intérprete de Libras” se o contexto envolver vídeos.
* **Validação contínua:** após ajustes, recompute as similaridades para verificar aumento de alinhamento.

---

## 6) Entregáveis

* **a) Planilha com vocabulário e vetores:** matriz TF da Seção 2.
* **b) Cálculos das similaridades:** detalhamento da Seção 3 (produtos escalares, normas e cosenos).
* **c) Resposta final:** F7 é a pior; F6 é a mais próxima das práticas (principalmente de BP5).

---

## 7) Referências rápidas (conceito)

* **Similaridade de cosseno:** razão entre o produto escalar e o produto dos comprimentos dos vetores.
* **Boas práticas de acessibilidade (exemplos):** legendas em vídeos, janela/intérprete de Libras, descrição de imagens (alt text), leitura por leitores de tela.

*(Fim do relatório.)*
