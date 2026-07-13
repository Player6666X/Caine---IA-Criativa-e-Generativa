# PROJETO: MOTOR DE SÍNTESE E CRIAÇÃO LÓGICA (PROTÓTIPO 2D)
## *Arquitetura de IA Simbólica-Neuro Inspirada no "Caine"*

**Versão:** 1.0 (Protótipo conceitual)
**Objetivo:** Criar um núcleo de inteligência criativa e generativa em ambiente 2D, capaz de raciocinar, decompor imagens, criar formas inéditas e aprender novos conceitos, utilizando **exclusivamente** processamento determinístico e bancos de dados simbólicos, sem depender de GPUs pesadas ou redes neurais com bilhões de parâmetros.

---

### 1. FILOSOFIA CENTRAL: SIMBÓLICA-NEURO (Inversão de Paradigma)

Ao invés de usar redes neurais pesadas (que armazenam conhecimento em trilhões de pesos decimais como `0,78453...`), utilizamos um banco de dados de **caracteres e números inteiros** (Simbólico). 

- **Peso vira ID:** Um conceito como "Círculo" não é uma probabilidade, e sim um caractere (`ID_CIRCULO = 001`).
- **Correlação vira Aresta:** A relação entre "Círculo" e "Raio" é armazenada como um traço (aresta) em um grafo.
- **Vantagem:** Caracteres e IDs ocupam poucos bytes na memória, tornando todo o sistema extremamente leve e rápido, sem perder a precisão lógica.

---

### 2. A BASE DE CONHECIMENTO: A "TEIA" (GRAFO DE CONCEITOS)

Todo o conhecimento da IA é armazenado em um **Grafo Acíclico Direcionado (ou rede semântica)**.

- **Nós (Vértices):** Representam conceitos absolutos. Ex: `"CIRCULO"`, `"VERMELHO"`, `"ROTACAO_45"`, `"SOMBRA"`.
- **Arestas (Traços):** Representam domínios ou relações. Ex: `["CIRCULO"] ---[POSSUI_COR]---> ["VERMELHO"]`.
- **Aprendizado:** Quando a IA descobre algo novo (ex: uma flor), ela cria um novo nó chamado `"FLOR"` e o conecta aos nós existentes (`"PETALA"`, `"SIMETRIA_RADIAL"`). Isso é feito por **Unificação de Padrões**, não por retropropagação.

---

### 3. VISÃO E PRÉ-PROCESSAMENTO (SEM REDES NEURAIS)

Para "enxergar" uma imagem de entrada, o sistema não usa redes convolucionais (CNNs). Em vez disso, ele divide a imagem em **3 camadas lógicas** utilizando algoritmos matemáticos clássicos:

1. **Camada Pixel (Contorno):** 
   - Usa o detector de bordas **Canny** e o algoritmo **Potrace** (ou cadeia de Freeman).
   - Resultado: A imagem é transformada em uma lista de **curvas vetoriais (Bézier)**. Ex: `Curva_Inicio(X,Y) -> Curva_Controle -> Curva_Fim`.
   
2. **Camada Coloração (Domínio Lógico):**
   - Usa **K-Means** para reduzir a paleta de cores a, no máximo, 5 cores dominantes.
   - Cada cor dominante é "batizada" no banco de dados (ex: se o RGB for próximo de laranja, vira o símbolo `COR_LARANJA`).
   - O resultado é: *"A Região delimitada pela Curva_1 possui o atributo COR_LARANJA"*.

3. **Camada Layer (Profundidade / Z-Index):**
   - Para saber o que está na frente, o sistema usa a **Ordem de Oclusão por Toque de Bordas**.
   - Se a borda do Objeto A interrompe a borda do Objeto B, e a cor de B continua atrás da borda de A, o sistema define automaticamente: `OBJETO_A -> LAYER: 1` e `OBJETO_B -> LAYER: 0`.
   - Resultado: Profundidade resolvida sem IA generativa.

---

### 4. CICLO DE APRENDIZADO E GERAÇÃO (A "MUTAÇÃO LÓGICA")

Quando a IA recebe uma imagem para aprender ou um comando para criar, ela executa o seguinte fluxo determinístico:

1. **Decomposição:** O pré-processamento (etapa 3) decompõe a imagem alvo em um **Grafo Alvo** de formas complexas e básicas.
2. **Tentativa de Geração:** O motor de criação começa com formas básicas (círculos, elipses, retângulos) e **muta** seus parâmetros (raio, rotação, posição, cor) aleatoriamente, mas seguindo as regras matemáticas do Grafo base.
3. **Comparação (Avaliação):** Ao invés de comparar pixel a pixel, o sistema calcula a **Distância de Edição entre Grafos**. Ou seja, ele conta quantos nós e arestas precisam ser trocados/adicionados para o Grafo Gerado se igualar ao Grafo Alvo.
4. **Critério de Parada (A Armadilha dos 100%):** 
   - Determinamos que o sistema **NUNCA** deve buscar 100% de fidelidade (para evitar explodir a memória com 10 milhões de círculos para fazer um gato peludo).
   - **Regra de Ouro:** O sistema itera e ajusta a mutação até que a Distância de Edição fique estagnada (sem melhorar) por **10 tentativas consecutivas**. Nesse ponto, ele para e salva o melhor resultado.

---

### 5. CRIAÇÃO DE FORMAS COMPLEXAS (ABSTRAÇÃO INTELIGENTE)

Se durante as tentativas a IA perceber que só formas básicas não resolvem (ex: textura de pelo ou grama), ela ativa o **Módulo de Síntese Abstrata**:

- Ela cria um **novo nó complexo** no banco de dados, com um nome simbólico (ex: `"PELO_AGRUPADO"`).
- A definição desse nó não são pixels, mas uma **fórmula matemática**: `{Repetir_Elipse_Distorcida(vezes=30, raio_variavel=2px, rotacao_aleatoria)}`.
- A partir de agora, quando a IA vir uma imagem com aquela textura, ela não tentará decifrar fio por fio. Ela puxa o conceito `"PELO_AGRUPADO"` do banco e o aplica inteiro, gerando a textura em milissegundos.

---

### 6. INICIATIVA E CRIATIVIDADE (O "MOTOR DE RECOMPENSA INTRÍNSECA")

Para que a IA não fique só copiando, mas também **crie coisas inéditas** (como o Caine), implementamos um sistema de recompensa baseado em **Novidade**:

- A IA está em um loop infinito de geração.
- Cada forma criada passa por um "Crítico Interno" que calcula:
  1. **Validade:** A forma respeita as regras geométricas do banco de dados? (Ex: um círculo deve fechar).
  2. **Novidade:** Qual a distância (no grafo) entre essa nova forma e tudo o que ela já criou antes?
- A recompensa máxima é dada quando a forma é **Válida** e **Altamente Nova**.
- Quando ela encontra uma combinação nova e válida (ex: conectar `"ESPINHO"` + `"CIRCULO"` para gerar `"RODA_DENTADA"`), ela automaticamente adiciona esse novo nó ao Grafo global para uso futuro.

---

### 7. ESTRUTURA TÉCNICA SUGERIDA PARA O PROTÓTIPO

Para colocar esse projeto em código agora, você precisará das seguintes bibliotecas leves (todas rodam em CPU):

- **Python 3.x** (Linguagem principal).
- **OpenCV (ou Pillow):** Para a captura da imagem e aplicação do Canny Edge Detector.
- **Shapely:** Para manipulação e validação das formas geométricas (círculos, polígonos).
- **NetworkX:** Para construir, navegar e calcular a Distância de Edição no Grafo de Conceitos.
- **Manim ou Matplotlib:** Para renderizar as formas criadas na tela (ambiente 2D).

---

### 8. CONCLUSÃO DO PROTÓTIPO

Este projeto entrega uma IA que:
- **Raciocina** através de lógica de grafos, não por "achismo" estatístico.
- **Enxerga** através de filtros matemáticos (bordas, cores e camadas).
- **Aprende** criando novos nós e fórmulas, sem precisar de backpropagation.
- **Cria** com iniciativa própria, buscando ativamente por novidades dentro das regras estabelecidas.

É um motor de criatividade sintética, determinístico, explicável, e leve o suficiente para rodar em qualquer computador pessoal, dando os primeiros passos sólidos em direção a uma inteligência artificial verdadeiramente lógica e inventiva.

**Fim da especificação do protótipo.**
