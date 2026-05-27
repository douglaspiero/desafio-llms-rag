# RAG — Os Sertões (Euclides da Cunha)

Três abordagens de Recuperação e Geração de Respostas (RAG) aplicadas ao livro **"Os Sertões"** de Euclides da Cunha.

---

## Estrutura do repositório

```
rag-os-sertoes/
├── notebook_1_naive_rag.ipynb      # Abordagem 1: Naive RAG
├── notebook_2_parent_rag.ipynb     # Abordagem 2: Parent RAG
├── notebook_3_rerank_rag.ipynb     # Abordagem 3: Rerank RAG
├── .env.example                    # Exemplo de variáveis de ambiente
├── .gitignore
└── README.md
```

---

## Abordagens implementadas

### 1. Naive RAG
- Chunks de tamanho fixo (1000 chars) com sobreposição de 200 chars
- Busca por similaridade de cosseno via ChromaDB
- Geração direta com os top-5 chunks recuperados

### 2. Parent RAG
- **Chunks filhos** pequenos (~300 chars) → usados para busca precisa
- **Chunks pais** grandes (~1500 chars) → usados como contexto rico na geração
- Cada filho aponta para seu pai; após a busca, recupera o texto completo do pai

### 3. Rerank RAG
- Retrieval largo: bi-encoder recupera top-20 candidatos
- Reranking: cross-encoder (`mmarco-mMiniLMv2`) pontua cada par `(query, chunk)`
- Geração com apenas os top-5 após reordenação por relevância real

---

## Pré-requisitos

- Python 3.9+
- Conta na [Anthropic](https://console.anthropic.com/) com chave de API

---

## Instalação

```bash
# Clone o repositório
git clone https://github.com/<SEU_USUARIO>/rag-os-sertoes.git
cd rag-os-sertoes

# Crie e ative um ambiente virtual
python -m venv venv
source venv/bin/activate          # Linux/Mac
# venv\Scripts\activate           # Windows

# Instale as dependências
pip install anthropic pypdf chromadb sentence-transformers \
            langchain langchain-community tiktoken matplotlib
```

---

## Configuração da API Key

Crie um arquivo `.env` na raiz do projeto (nunca commite este arquivo):

```bash
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Ou exporte diretamente no terminal antes de abrir o Jupyter:

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
jupyter notebook
```

---

## Como executar

Abra cada notebook no Jupyter e execute as células em ordem:

```bash
jupyter notebook notebook_1_naive_rag.ipynb
jupyter notebook notebook_2_parent_rag.ipynb
jupyter notebook notebook_3_rerank_rag.ipynb
```

O PDF é baixado automaticamente na primeira execução.

---

## Questões respondidas pelos modelos

1. Qual é a visão de Euclides da Cunha sobre o ambiente natural do sertão nordestino e como ele influencia a vida dos habitantes?
2. Quais são as principais características da população sertaneja descritas por Euclides da Cunha? Como ele relaciona essas características com o ambiente em que vivem?
3. Qual foi o contexto histórico e político que levou à Guerra de Canudos, segundo Euclides da Cunha?
4. Como Euclides da Cunha descreve a figura de Antônio Conselheiro e seu papel na Guerra de Canudos?
5. Quais são os principais aspectos da crítica social e política presentes em "Os Sertões"? Como esses aspectos refletem a visão do autor sobre o Brasil da época?

---

## Dependências principais

| Biblioteca | Uso |
|---|---|
| `anthropic` | LLM para geração de respostas (Claude) |
| `pypdf` | Extração de texto do PDF |
| `chromadb` | Banco de dados vetorial |
| `sentence-transformers` | Bi-encoder (embeddings) e cross-encoder (reranking) |
| `matplotlib` | Visualização dos scores de reranking |
