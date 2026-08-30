# Librify Reader 📚

O **Librify Reader** é um leitor de e-books e documentos moderno, de alta performance e acessível para Android. Desenvolvido nativamente em **Kotlin** com **Jetpack Compose**, o projeto segue rigorosamente os padrões de **Clean Architecture** e oferece uma experiência de leitura premium inspirada nos melhores e-readers e aplicativos de leitura do mercado, enriquecida com Inteligência Artificial e ferramentas de estudo ativo.

---

## 🚀 Recursos em Destaque

### 📖 1. Mecanismo de Leitura Premium & Tipografia
* **Estética Visual Refinada:** Ritmo vertical otimizado com espaçamento entrelinhas expandido para `1.6f` e quebras de linha duplas (`\n\n`) que garantem respiro aos parágrafos, eliminando o cansaço de "blocos infinitos de texto".
* **Margens Generosas:** Padding lateral de `32.dp` emulando as proporções físicas de um livro impresso.
* **Detecção Inteligente de Títulos:** Identificação de tags HTML (`<h1>` a `<h6>`) em arquivos EPUB e centralização automática em negrito com cores dinâmicas ligadas ao tema.
* **Exibição Inteligente de Imagens:** Renderização assíncrona de ilustrações (EPUB) integrada no fluxo de texto usando a biblioteca **Coil** via `inlineContent`, com alinhamento vertical `AboveBaseline` para evitar sobreposição de textos.
* **Preservação de Posição de Leitura Tridimensional:** O algoritmo de retomada inteligente utiliza um sistema de redundância:
  * *Prioridade Visual:* Mantém a subpágina exata se o layout não mudou.
  * *Fall-back Lógico:* Caso o tamanho ou estilo de fonte seja alterado, o app recalcula a posição dinamicamente com base em um índice lógico em tempo real para evitar saltos.

### 🧠 2. Assistente de IA Integrado (Gemini AI)
* **Consultas Diretas no Fluxo de Texto:** Selecione qualquer termo para acionar ferramentas instantâneas de **Dicionário** ou **Tradução** via API do Gemini (modelo otimizado `gemini-1.5-flash`), exibidas em um painel inferior (`ModalBottomSheet`).
* **Interação Fluida & Humanizada:** Respostas em tempo real via streaming que eliminam telas de carregamento travadas, com filtro inteligente para remover mensagens de saudação redundantes antes do salvamento local.
* **Histórico do Assistente Global & Local:** Banco de dados offline para consultas passadas. O histórico exibe a página em que a pergunta foi realizada, permite renomear consultas e realizar compartilhamento nativo.
* **Respostas Selecionáveis:** Suporte completo à cópia parcial ou total do texto gerado pela IA no leitor, histórico local ou histórico global utilizando `SelectionContainer`.

### 🎨 3. Conforto Visual & Temas Personalizados
* **Modo Papiro (Sépia Premium):** Paleta relaxante com fundo creme suave (`#F4ECD8`) e fonte café escuro (`#5B4636`), reduzindo drasticamente a emissão de luz azul.
* **Modo Noite OLED:** Fundo preto absoluto para economia de bateria em telas AMOLED e conforto no escuro total.
* **Temas Claro & Escuro Suavizados:** Tons de cinza balanceados substituindo contrastes puros para evitar a fadiga visual.
* **Configurações Individuais por Livro:** Preferências de fonte (Sans Serif, Serif, Monospace, Cursive) e tamanho de texto são persistidas de forma independente para cada livro no banco de dados.

### ✏️ 4. Ferramentas de Estudo Ativo (Librify Study Pro)
* **Anotações de "Post-it" em Destaques:** Crie, visualize e edite notas de estudo diretamente sobre trechos marcados.
* **Indicador Discreto de Notas:** Palavras que possuem anotações anexas têm sua última letra renderizada com peso extra-negrito (**bold**) na interface do leitor, oferecendo um sinalizador limpo que não polui o texto.

### 📊 5. Painel de Estatísticas de Leitura
* **Rastreamento em Tempo Real:** Monitora o tempo ativo de leitura e contabiliza as palavras processadas.
* **Filtros Anti-Duplicidade:** Atraso controlado de 1,5 segundos no motor de contagem para evitar inflar estatísticas durante folheadas rápidas ou rotações de tela frequentes.
* **Métricas Detalhadas (Stats Dialog):** Exibe velocidade média (WPM), progresso da meta de leitura diária, total de palavras lidas hoje e estimativa dinâmica de tempo para finalizar o capítulo.

### 💎 6. Monetização & Gestão de Assinaturas (Librify Pro)
* **Transição de Modelo:** Sistema automatizado via cobrança (faturamento integrado ao ciclo de vida do app) integrado ao ciclo de vida do app para planos recorrentes (Mensal: `librify_monthly_pro` e Anual: `librify_annual_pro`).
* **Compatibilidade Retroativa:** Preserva direitos adquiridos por apoiadores históricos que possuíam IDs antigos de compra única (`vip_support` / `librify_pro` vitalício), concedendo automaticamente o status PRO.
* **Degustação Diária de IA:** Usuários da versão gratuita recebem 3 usos diários do Assistente de IA, controlados localmente via `SharedPreferences` de forma segura.

---

## 🛠️ Stack Tecnológico

O Librify Reader utiliza as melhores práticas e bibliotecas modernas recomendadas pelo ecossistema Android:

* **UI & Estilização:** Jetpack Compose, Material Design 3, Coil (carregamento de imagens), Navigation Compose.
* **Arquitetura:** Clean Architecture (Domain, Data, Presentation/UI) aliada a padrões reativos unidirecionais (MVI / MVVM).
* **Processamento Assíncrono:** Kotlin Coroutines & Flow (com gerenciamento de estado via `StateFlow` e `SharedFlow`).
* **Banco de Dados (Persistência):** Room Database com controle estrito de versões e migrações seguras (atualizado para a versão 9 para acomodar `ReadingStats`, `AiResponse` e preferências do leitor).
* **Injeção de Dependências:** Hilt (Dagger) para desacoplamento de serviços e testabilidade.
* **IA:** SDK do Google AI Client (Gemini API Integration).
* **Faturamento:** cobrança (faturamento integrado ao ciclo de vida do app) Library para gerenciar compras e assinaturas (In-App Purchases & Subscriptions).

---

## 📐 Arquitetura de Dados & Migrações do Banco de Dados

Para garantir a evolução estável do aplicativo e preservar os dados dos usuários, o banco de dados Room do Librify Reader utiliza um pipeline robusto de atualizações estruturais:

* **Versão 4:** Introdução da entidade `AiResponse` para armazenamento local de consultas ao Assistente, implementando chave estrangeira com deleção em cascata (`CASCADE`) ligada ao livro correspondente.
* **Versão 7:** Adição de campos de preferência individualizada (`fontSize` e `fontFamily`) diretamente na entidade `Book` para carregar o estilo de leitura automaticamente.
* **Versão 9:** Criação da entidade `ReadingStats` para suporte ao painel de acompanhamento de estudo (tempo decorrido e palavras processadas agrupadas por data).

---

## 📬 Contato

Caso tenha dúvidas, sugestões ou precise de suporte sobre o Librify Reader, entre em contato através das *Issues* do GitHub ou pelo e-mail do desenvolvedor.
