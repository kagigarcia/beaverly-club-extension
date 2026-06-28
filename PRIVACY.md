# Política de Privacidade — Beaverly Companion

**Última atualização:** 28 de junho de 2026

## TL;DR

- ✅ A Extensão **não coleta dados pessoais sensíveis** (nome, CPF, cartão, senha, localização).
- ℹ️ Ela envia para a **sua própria conta Beaverly** apenas o que **você escolhe salvar** (página, trecho, livro, progresso) — sempre mediante uma ação explícita sua.
- ✅ **Não compartilha dados com terceiros** (sem Google Analytics, Mixpanel, Sentry, OpenAI, etc.). O único destino é a API do Beaverly.
- ✅ **Não captura nada automaticamente** e **não usa `<all_urls>`** — só lê a aba quando você clica.
- ✅ O código de conexão fica no seu navegador; o servidor guarda apenas o **hash** dele, e você pode **revogá-lo a qualquer momento**.

---

## 🔍 Quais dados a extensão acessa e envia?

A Extensão só age **mediante uma ação explícita sua** (clique no popup ou em um item do menu de contexto). Não há leitura em segundo plano nem em outras abas.

### Página salva (“Salvar página” / “Ler depois”)

Ao salvar uma página, a Extensão lê e envia para o Beaverly: **URL, título, descrição, imagem de capa, nome do site e o tipo** (página/artigo/PDF). Serve para criar o item “ler depois” na sua conta.

### Trecho selecionado (salvar trecho / nota / citação / flashcard / enviar a clube)

Ao acionar uma dessas ações sobre um texto que **você selecionou**, a Extensão envia **o trecho selecionado** (sanitizado e com limite de tamanho) para o Beaverly, para virar um item salvo, uma nota/citação de um livro da sua estante, um flashcard de estudo, ou uma discussão em um clube que você escolher.

### Livro detectado

Em páginas de livros (ex.: Amazon, Goodreads, Skoob, Google Books, Open Library, Project Gutenberg), a Extensão extrai **título, autor e ISBN** a partir dos metadados públicos da página para: adicionar o livro à sua estante e procurar uma **versão gratuita** em acervos públicos.

### Progresso de leitura

Em PDFs, a Extensão envia **a página atual e o total de páginas que você digita** para registrar seu progresso em um livro da sua estante.

**A Extensão NÃO lê nem envia:** senhas, cookies, tokens de outros sites, histórico de navegação, conteúdo de outras abas, nem qualquer dado de páginas que você não tenha mandado salvar.

---

## 🔒 Permissões utilizadas

| Permissão | Por que é necessária |
|-----------|----------------------|
| `storage` | Guardar localmente o código de conexão, o cache da sessão (token de acesso de curta duração) e a preferência de idioma. |
| `activeTab` | Ler a aba atual **apenas quando você aciona a extensão**, para extrair título/URL/metadados/seleção da página que você quer salvar. |
| `scripting` | Executar a coleta de dados da página **sob demanda** (após sua ação), na aba ativa. Nenhum script é injetado automaticamente. |
| `contextMenus` | Adicionar os itens de menu de contexto (“Salvar página”, “Salvar trecho”, “Criar flashcard”, “Buscar este livro no Beaverly”). |
| `host_permissions` | Limitado às origens do **próprio Beaverly** (`https://beaverly.com.br/*` e `http://localhost:5173/*` para desenvolvimento) — para o handoff de conexão e as chamadas à API. |

**Sem `<all_urls>`.** A leitura de páginas de terceiros ocorre via `activeTab` + `scripting`, sempre sob ação explícita sua.

---

## 💽 Armazenamento e localização dos dados

| Tipo de dado | Onde mora | Identificável? |
|---|---|---|
| Código de conexão (token) | `chrome.storage.local` (só no seu navegador) | ❌ |
| Cache da sessão (JWT curto) + expiração | `chrome.storage.local` | ❌ |
| Preferência de idioma | `chrome.storage.local` | ❌ |
| Itens que você salva (páginas, trechos, notas, citações, progresso) | **Sua conta no servidor Beaverly** (`beaverly.com.br`) | ✅ vinculado à sua conta |

Os itens salvos ficam na **sua conta Beaverly** e são regidos também pela **Política de Privacidade da plataforma Beaverly**: <https://beaverly.com.br/privacy>.

---

## 🔑 Autenticação

A conexão usa um **código opaco** gerado por você em *Configurações → Extensão* no Beaverly. Esse código funciona como uma senha:

- É guardado **somente no seu navegador** (`chrome.storage.local`).
- No servidor, é armazenado **apenas o hash** (SHA-256) — nunca o código em si.
- A cada uso, o código é trocado por um **token de acesso de curta duração** (JWT). As chamadas usam `Authorization: Bearer` (sem cookies).
- O código **nunca é registrado em logs** e pode ser **revogado a qualquer momento** em *Configurações → Extensão*, invalidando a próxima sessão.

---

## 📤 Compartilhamento de dados

A Extensão **NÃO compartilha dados com terceiros externos**.

- ✅ O único destino das chamadas de rede é a **API do Beaverly** (sua conta).
- ❌ **Nenhum serviço de analytics terceiro** (Google Analytics, Mixpanel, Sentry, Amplitude, etc.).
- ❌ **Nenhuma API de IA externa** (OpenAI, Anthropic, Gemini, etc.) é chamada pela Extensão.
- ❌ **Nenhuma venda** de dados e **nenhum uso para crédito/empréstimo**.

---

## 🌐 Internacionalização

A Extensão está disponível em **8 idiomas** (português, inglês, espanhol, francês, alemão, italiano, japonês e chinês). A preferência é salva localmente e nunca compartilhada.

---

## ❌ Seus direitos de privacidade (exclusão de dados)

- **Revogar o acesso:** *Configurações → Extensão* no Beaverly → revogar a conexão.
- **Excluir itens salvos:** na página **“Salvos”** (`/salvos`) do Beaverly você pode arquivar ou remover seus itens.
- **Apagar dados locais:** desinstale a Extensão; para limpar o que ficou no navegador, em uma aba `beaverly.com.br` abra DevTools → *Application* → *Storage* → *Clear site data*, ou no Console:

  ```js
  Object.keys(localStorage).forEach((k) => { if (k.startsWith('beaverly.companion.')) localStorage.removeItem(k) })
  ```

- **Excluir a conta:** pelas configurações da sua conta Beaverly.

---

## 🔓 Transparência e código

Esta extensão segue as diretrizes da Chrome Web Store e melhores práticas de segurança.

O código é entregue **minificado** por motivos de tamanho de bundle e proteção de propriedade intelectual. **A Extensão NÃO utiliza ofuscação de código**, em conformidade com a [Política de Legibilidade de Código](https://developer.chrome.com/docs/webstore/program-policies/code-readability) da Chrome Web Store.

Você pode **verificar de forma independente** monitorando o tráfego de rede da extensão (que mostrará apenas chamadas para a API do Beaverly) e inspecionando o `chrome.storage.local` via DevTools.

---

## 📬 Contato

Para dúvidas sobre privacidade, solicitações de exclusão de dados ou reporte de problemas de segurança:

**Kägi Adrian Garcia** — desenvolvedor responsável
✉️ contato@giindevs.com.br

Ou pelo formulário “Fale conosco” dentro do Beaverly.
