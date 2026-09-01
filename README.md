# 📖 Bíblia App

Aplicativo de estudo bíblico com inteligência artificial, para Android (via Capacitor). Funciona majoritariamente offline — a leitura da Bíblia não depende de internet — e usa IA (Groq) para explicações, esboços, lições de EBD e dicionário.

---

## ✨ Funcionalidades

### 📖 Leitura
- **Tradução Brasileira (TB)** completa — 66 livros, embutida no app, domínio público confirmado pela própria SBB. Nunca depende de internet.
- **Hebraico** (Texto Massorético / Westminster Leningrad Codex, domínio público) — só Antigo Testamento.
- **Grego** (Nestle 1904, domínio público) — só Novo Testamento.
- Alternância entre os três com um toque, sem downloads.

### ✨ Me Explica (IA)
Toque no ícone ✨ em qualquer versículo para uma explicação gerada por IA: contexto histórico, significado e aplicação prática. Resultado é salvo localmente (SQLite) — não gasta uma nova consulta se você abrir o mesmo versículo de novo.

### 🔤 Idioma Original (IA)
Toque no ícone 🔤 para ver uma análise do texto original (hebraico ou grego, conforme o testamento): texto na escrita original, transliteração, tradução literal palavra por palavra e palavras-chave. Gerado por IA — recomendado cruzar com uma fonte crítica de referência para uso sério de pregação/estudo aprofundado.

### ⋯ Grifos, Favoritos e Compartilhar
Toque no ícone ⋯ em qualquer versículo para:
- **Grifar** com uma de 5 cores (toque de novo na mesma cor para remover)
- **Favoritar** (★)
- **Compartilhar** o versículo via o menu nativo do Android

### 📝 Gerador de Esboços
Gera esboços de pregação/estudo a partir de uma passagem ou tema, com 28 "lentes" de análise teológica à escolha (Cristologia, Escatologia, Tipologia Bíblica, Ética Cristã, etc.).

### 📘 EBD
Gera lições completas de Escola Bíblica Dominical (texto áureo, verdade central, comentário, perguntas de discussão, aplicação prática), adaptadas ao público (adultos, jovens, adolescentes, crianças).

### 📔 Dicionário Bíblico
Busca de termos, nomes e conceitos bíblicos com definição via IA (significado, ocorrência, peso teológico). Cache local por termo.

### 🔍 Concordância
Busca de palavras em toda a Bíblia (Tradução Brasileira). 100% local e instantânea — não usa API nem internet.

### ⭐ Mais (Favoritos, Grifos e Planos de Leitura)
- **Favoritos** e **Grifos**: listas de tudo que você marcou, com atalho para voltar ao versículo.
- **Planos de leitura**: 4 planos prontos (Evangelhos em 20 dias, Salmos em 30 dias, Novo Testamento em 60 dias, Bíblia completa em 1 ano). Ao iniciar um plano, o progresso é rastreado **automaticamente conforme você rola a página** durante a leitura — sem precisar marcar nada manualmente. Mostra percentual de capítulos lidos e um checklist visual por dia.

### 📤 Compartilhar estudos
Botão de compartilhar nos resultados de Esboços, EBD, Dicionário, Me Explica e Idioma Original — abre o menu nativo do Android (WhatsApp, e-mail, etc.), com fallback para copiar ao clipboard se o compartilhamento nativo não estiver disponível.

---

## 🧩 O que exige internet (e o que não exige)

| Funciona 100% offline | Precisa de internet |
|---|---|
| Leitura (TB, Hebraico, Grego) | Me Explica (gera conteúdo novo) |
| Concordância | Idioma Original (gera conteúdo novo) |
| Grifos, Favoritos, Planos de leitura | Esboços |
| Conteúdo de IA já gerado antes (fica em cache) | EBD |
| | Dicionário (termo novo) |

---

## 🛠️ Stack técnica

- **Capacitor 6** (Android nativo a partir de HTML/CSS/JS puro, sem framework)
- **SQLite** local (`@capacitor-community/sqlite`) para cache de IA, grifos, favoritos e progresso de planos — com fallback automático em memória se o plugin não estiver disponível
- **Groq API** (modelo `openai/gpt-oss-120b`) para todas as funções de IA
- **@capacitor/share** para compartilhamento nativo

---

## 📁 Estrutura do projeto

```
biblia-app/
├── android-icons/          # ícones prontos (mipmap), aplicados via script
├── scripts/
│   ├── patch-android.js    # aplica config de desugaring exigida pelo SQLite
│   └── apply-icons.js      # copia os ícones pro projeto Android
├── www/
│   ├── index.html
│   ├── css/style.css
│   ├── bible-data/
│   │   ├── books-meta.json # metadados dos 66 livros (nome, capítulos, testamento)
│   │   ├── tb/              # Tradução Brasileira, 1 JSON por livro
│   │   ├── heb/              # hebraico (Antigo Testamento)
│   │   └── grc/              # grego (Novo Testamento)
│   └── js/
│       ├── config.js         # chave da Groq e configs gerais
│       ├── db.js             # camada de persistência (SQLite + fallback)
│       ├── app.js            # leitura, Me Explica, progresso de planos
│       ├── verse-actions.js  # popup de grifar/favoritar/compartilhar
│       ├── original-language.js
│       ├── outlines.js       # Esboços
│       ├── ebd.js
│       ├── dictionary.js
│       ├── concordance.js
│       ├── reading-plans.js  # definição dos planos de leitura
│       ├── mais.js           # aba Favoritos/Grifos/Planos
│       ├── theological-lenses.js
│       ├── share.js
│       └── tabs.js
├── capacitor.config.json
└── package.json
```

---

## 🚀 Como rodar

### Primeira vez (projeto Android ainda não existe)
```bash
npm install
npx cap add android
npm run icons
npm run sync
npx cap open android
```

### Depois de qualquer mudança no código
```bash
npm run sync
```
(sincroniza os arquivos web + reaplica a config do SQLite automaticamente, mesmo que o Capacitor regenere o `build.gradle`)

### Só abrir o Android Studio
```bash
npx cap open android
```

**Antes de rodar pela primeira vez**, preencha sua chave da Groq em `www/js/config.js`:
```js
GROQ_API_KEY: "sua_chave_aqui",
```
Gere uma chave gratuita em [console.groq.com/keys](https://console.groq.com/keys).

---

## ⚠️ Limitações conhecidas

- **Idioma Original** e traduções de idioma original são **gerados por IA**, não vêm de uma fonte crítica fixa — confira num interlinear de referência antes de usar em pregação.
- Se o SQLite não inicializar no aparelho, o app cai para um cache **em memória** (não persiste entre reaberturas do app) — grifos/favoritos/progresso de planos podem se perder nesse caso.
- Traduções com direitos autorais ativos (NVI, ACF, RA, NAA etc.) **não podem ser embutidas** no app — só a Tradução Brasileira (TB), hebraico e grego (todos domínio público) estão disponíveis offline.
