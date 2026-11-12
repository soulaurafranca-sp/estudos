# Estudos de Idiomas — versão GitHub Pages

Apps de estudo diário (Inglês KNN Book 1 e Francês TEF/TCF) com:

- **Login com Google:** cada pessoa tem seu progresso, sincronizado sozinho entre computador e celular.
- **Correções com IA (gratuitas, via Gemini):** escrita livre, tarefas do TEF/TCF e o botão "Por que errei?" nos exercícios.

São 3 passos. O 1 e o 2 são obrigatórios para sincronizar; o 3 liga as correções com IA.

---

## Arquivos

| Arquivo | Para quê |
|---|---|
| `index.html` | Página inicial (escolher o idioma) |
| `ingles.html` / `frances.html` | Os apps |
| `config.js` | **Único arquivo que você edita** |
| `firestore.rules` | Regras de segurança do banco (colar no Firebase) |
| `apps-script/Codigo.gs` | Código das correções com IA (colar no Apps Script) |

---

## Passo 1 — Firebase: login e banco de dados (≈ 10 min, gratuito)

1. Entre em **console.firebase.google.com** com sua conta Google e clique em **Criar projeto** (ou *Adicionar projeto*). Nome: `estudos-idiomas`. Pode desativar o Google Analytics.
2. Na página do projeto, clique no ícone **`</>` (Web)** para adicionar um app. Apelido: `site`. Não marque Firebase Hosting. Clique em **Registrar app**.
3. Vai aparecer um bloco `const firebaseConfig = { apiKey: "...", authDomain: "...", ... }`. **Copie os valores** para o `config.js`, dentro de `firebase: { ... }`.
4. Menu **Build → Authentication → Começar** → aba **Método de login** → **Google** → **Ativar** → escolha o e-mail de suporte → **Salvar**.
5. Ainda em Authentication → **Configurações → Domínios autorizados → Adicionar domínio** → digite `SEU-USUARIO.github.io` (o mesmo endereço do seu GitHub Pages, sem `https://` e sem a pasta).
6. Menu **Build → Firestore Database → Criar banco de dados** → local **southamerica-east1 (São Paulo)** → **modo de produção** → Criar.
7. Na aba **Regras** do Firestore, apague o que estiver lá, cole o conteúdo do arquivo `firestore.rules` e clique em **Publicar**.

O plano gratuito (Spark) sobra para uso familiar.

## Passo 2 — GitHub Pages (≈ 5 min)

1. No GitHub, crie um repositório, por exemplo `estudos`.
2. Envie **todos os arquivos desta pasta**, incluindo a pasta `apps-script` e o `config.js` já preenchido (*Add file → Upload files*).
3. **Settings → Pages → Branch: `main` / `(root)` → Save.**
4. Em 1 ou 2 minutos o site fica em `https://SEU-USUARIO.github.io/estudos/`.

Abra, clique em **Entrar com Google** e crie seu perfil. No celular, abra o mesmo endereço, entre com a **mesma conta Google** e use "Adicionar à tela inicial".

> O `apiKey` do Firebase pode ficar público no site: ele só identifica o projeto. Quem protege os dados são as regras do passo 1.7, que deixam cada pessoa ler e gravar **só o próprio progresso**.

## Passo 3 — Correções com IA (opcional, gratuito)

As correções usam a **faixa gratuita da API Gemini, do Google**: não pede cartão nem cobra nada. Ela tem um limite diário de pedidos por projeto; o app ainda limita as correções por pessoa (padrão 30 por dia) para a cota não acabar. Na faixa gratuita, o Google pode usar os textos enviados para melhorar os produtos dele, então não escreva dados pessoais nas redações.

1. Entre em **aistudio.google.com** com sua conta Google → **Get API key → Criar chave de API**. Copie a chave (começa com `AIza`). **Não** ative faturamento: assim nunca há cobrança.
2. Abra **script.google.com → Novo projeto**. Apague o conteúdo, cole o arquivo `apps-script/Codigo.gs` e salve. Nome: `Correções Idiomas`.
3. Clique na **engrenagem (Configurações do projeto) → Propriedades do script → Adicionar propriedade**:
   - `GEMINI_API_KEY` = a chave do item 1
   - `FIREBASE_API_KEY` = o mesmo `apiKey` do `config.js`
   - opcional `EMAILS_LIBERADOS` = e-mails de quem pode usar, separados por vírgula (vazio = qualquer pessoa logada)
   - opcional `LIMITE_DIARIO` = correções por pessoa por dia (padrão 30)
4. No editor, selecione a função **`testarChave`** e clique em **Executar**. Autorize e confira no registro se aparece `funcionando`.
5. **Implantar → Nova implantação →** tipo **App da Web** → Executar como: **Eu** → Quem pode acessar: **Qualquer pessoa** → **Implantar**. Copie a **URL do app da Web** (termina em `/exec`).
6. Cole a URL em `aiProxy` no `config.js` e envie o arquivo de novo ao GitHub.

Se mudar o `Codigo.gs` depois, use **Implantar → Gerenciar implantações → Editar → Nova versão**. Assim a URL continua a mesma.

---

## Dúvidas comuns

- **"Este endereço não está autorizado no Firebase":** falta o passo 1.5.
- **O login abre e fecha sem entrar:** confira se o navegador não bloqueou o pop-up. No iPhone, use o Safari atualizado.
- **"As correções ainda não foram configuradas":** falta `aiProxy` no `config.js` ou as propriedades do passo 3.3.
- **"A cota gratuita de correções de hoje acabou":** o limite gratuito do Google foi atingido; ele renova no dia seguinte.
- **"Seu e-mail ainda não foi liberado":** inclua o e-mail em `EMAILS_LIBERADOS`.
- **Família na mesma conta Google:** cada pessoa cria seu perfil dentro da conta (sem senha entre perfis). Para privacidade total, cada pessoa usa a própria conta Google.
- **Microfone:** funciona no Chrome (computador e Android). No iPhone, o exercício de fala usa autoavaliação quando o reconhecimento não está disponível.
- **Atualizar o app:** substitua `ingles.html` / `frances.html` no repositório. O progresso fica no Firebase e não se perde.
