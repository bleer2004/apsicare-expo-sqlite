
# Apsicare — Prova de Conceito com Expo SQLite

Prova de conceito acadêmica desenvolvida para o TCC **Apsicare**, no contexto da disciplina de Desenvolvimento de Aplicativos Híbridos.

O objetivo é demonstrar como o **`expo-sqlite`** permite armazenar, consultar e recuperar dados de monitoramento de estresse **localmente no dispositivo**, mesmo sem conexão com a internet.

---

## 📋 Contexto do TCC

O **Apsicare** é um sistema de apoio ao monitoramento de saúde mental que utiliza dados multimodais provenientes de dispositivos vestíveis (smartwatches) para acompanhar indicadores relacionados ao estresse.

O projeto trabalha com:

- Dados biométricos coletados por wearables;
- Um indicador de estresse (`stress_score`) entre 0 e 1;
- Um aplicativo para acompanhamento dos dados;
- Inteligência artificial para auxiliar na interpretação das informações.

---

## ❓ Pergunta de pesquisa

> **Como o `expo-sqlite` permite armazenar e recuperar dados de monitoramento de estresse do Apsicare mesmo quando o dispositivo está sem conexão com a internet?**

---

## 🎯 Objetivo da prova de conceito

Construir um aplicativo mobile funcional, executável no **Expo Go**, que:

1. Registre medições simuladas de frequência cardíaca e `stress_score`;
2. Armazene essas medições em um banco SQLite local (`apsicare.db`);
3. Recupere o histórico mesmo sem internet;
4. Preserve os dados após fechar e reabrir o aplicativo;
5. Exiba os detalhes de cada medição.

A PoC **não** implementa autenticação, backend, integração real com smartwatch ou sincronização com servidor. Esses itens ficam como próximos passos no TCC.

---

## 🛠️ Tecnologias utilizadas

| Tecnologia | Uso |
|---|---|
| React Native | Interface mobile |
| Expo | Ambiente de desenvolvimento e execução no Expo Go |
| TypeScript | Tipagem estática |
| `expo-sqlite` | Banco de dados local (tecnologia investigada) |
| `expo-network` | Detecção do estado de conexão |
| React Navigation | Navegação entre as telas |
| React Hooks | Gerenciamento de estado e efeitos |

---

## 📱 Funcionalidades

### Tela 1 — Monitoramento
- Identidade visual Apsicare;
- Indicadores de BPM e `stress_score`;
- Classificação visual do nível de estresse (baixo, moderado, alto);
- Data e hora da última coleta;
- Botão **Registrar medição**;
- Indicador de conexão (Online / Modo offline).

### Tela 2 — Histórico
- Lista de todas as medições armazenadas;
- Ordenação do mais recente para o mais antigo;
- Atualização automática após uma nova medição.

### Tela 3 — Detalhes da medição
- Identificador;
- Frequência cardíaca;
- `stress_score`;
- Data e hora;
- Origem do registro (`simulated`).

### Tela 4 — Persistência e modo offline
- Contagem de registros no banco local;
- Instruções para testar o funcionamento sem internet;
- Limitações da PoC.

---

## 🗄️ Estrutura do banco

Banco: `apsicare.db`

Tabela `stress_records`:

| Campo | Tipo | Descrição |
|---|---|---|
| `id` | INTEGER PRIMARY KEY AUTOINCREMENT | Identificador |
| `recorded_at` | TEXT NOT NULL | Data e hora da medição (ISO 8601) |
| `heart_rate` | REAL NOT NULL | Frequência cardíaca (BPM) |
| `stress_score` | REAL NOT NULL | Indicador de estresse (0 a 1) |
| `source` | TEXT DEFAULT 'simulated' | Origem do registro |

Todas as consultas usam **queries parametrizadas** (`?`) para evitar injeção de SQL.

---

## 📂 Estrutura do projeto


apsicare-expo-sqlite/
├── App.tsx                  # SQLiteProvider + navegação
├── components/              # Componentes reutilizáveis
├── screens/                 # Telas do aplicativo
├── database/
│   └── database.ts          # Acesso ao SQLite
├── types/                   # Tipos TypeScript
├── utils/                   # Funções auxiliares
├── AGENTS.md                # Contexto do projeto para agentes de IA
├── CRITERIOS.md             # Critérios de aceitação
├── PESQUISA.md              # Diário de pesquisa
└── README.md


---

## ▶️ Como executar

### Pré-requisitos
- Node.js LTS (20 ou 22);
- Expo Go instalado no celular (Android ou iOS);
- Git (opcional).

### Passos

bash
# 1. Clonar o repositório
git clone https://github.com/bleer2004/apsicare-expo-sqlite.git
cd apsicare-expo-sqlite

# 2. Instalar dependências
npm install
npx expo install --fix

# 3. Rodar o projeto
npx expo start


Leia o QR Code:

- **Android:** abra o **Expo Go** e escaneie o código;
- **iOS:** abra a **câmera** e escaneie o código.

Se a rede bloquear a conexão, use:

bash
npx expo start -tunnel


---

## ✅ Critérios de aceitação

Os critérios estão detalhados em [`CRITERIOS.md`](./CRITERIOS.md):

- [ ] 1. Banco e tabela criados automaticamente na primeira execução.
- [ ] 2. Nova medição é salva e aparece no histórico.
- [ ] 3. Registros persistem após fechar e reabrir o app.
- [ ] 4. Histórico funciona sem conexão com a internet.
- [ ] 5. Detalhes da medição são exibidos corretamente.
- [ ] 6. Falhas no banco exibem mensagens de erro adequadas.

> Os checkboxes devem ser marcados **somente após a verificação no aparelho**. Veja os resultados reais em [`PESQUISA.md`](./PESQUISA.md).

---

## 📓 Diário de pesquisa

O arquivo [`PESQUISA.md`](./PESQUISA.md) registra:

- Contexto do TCC e problema investigado;
- Pergunta de pesquisa;
- Alternativas tecnológicas consideradas;
- Fontes oficiais consultadas;
- Funcionamento do `expo-sqlite`;
- Resultados dos testes;
- Limitações encontradas;
- **Onde a IA errou** (seção obrigatória da apresentação).

---

## ⚠️ Limitações desta PoC

- Os dados são **simulados** — não há uso de dados reais de pacientes.
- Não há sincronização com servidor nesta etapa.
- Não há autenticação nem integração real com smartwatch.
- A arquitetura foi preparada para, no futuro, adicionar uma fila de sincronização quando a conexão for restabelecida.

---

## 👤 Autor

- **Nome:** Ana Carolina Joaquim Januario 
- **Curso:** Ciências da computação
- **Instituição:** Instituto Mauá de Tecnologia 
- **Professor:** Rodrigo Bossini
- **Disciplina:** Desenvolvimento de Aplicativos Híbridos

---

## 📚 Referências

- Expo SQLite — https://docs.expo.dev/versions/latest/sdk/sqlite/
- Expo Network — https://docs.expo.dev/versions/latest/sdk/network/
- React Navigation — https://reactnavigation.org/docs/getting-started
- React Native — https://reactnative.dev/docs/getting-started
- React — https://react.dev


Se quiser, posso montar também o `README.md` em versão **mais curta** (para quem prefere algo enxuto) ou **em inglês**. Também posso te ajudar a escrever a seção "Onde a IA errou" do `PESQUISA.md` depois que você tiver o caso real.
