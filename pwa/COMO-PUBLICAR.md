# Amanda PWA — Como publicar e instalar no celular

Verô, segue o passo a passo. É chato uma vez só — depois é só abrir o ícone no celular.

## O que tem nessa pasta

```
pwa/
├── index.html          ← app de captura (manda texto + foto pra Amanda)
├── manifest.json       ← metadados pro celular tratar como app (inclui shortcut pro Painel)
├── service-worker.js   ← cache offline básico
├── icon-192.png        ← ícone do app (pequeno)
├── icon-512.png        ← ícone do app (grande)
├── painel/
│   └── index.html      ← painel completo (crianças, reforma, finanças, compras, wishlist)
└── COMO-PUBLICAR.md    ← este arquivo
```

## URLs após publicação

- **App de captura (principal):** https://helloveronicaponce.github.io/amanda/
- **Painel completo (ao vivo):** https://helloveronicaponce.github.io/amanda/painel/

O Painel acessa o Supabase diretamente — reforma e financeiro sempre ao vivo, sem precisar do desktop.

## Passo 1 — Criar o repositório no GitHub (5 min)

1. Vai em https://github.com/new
2. **Repository name:** `amanda`
3. **Owner:** helloveronicaponce
4. **Public** (precisa ser público pra GitHub Pages funcionar no plano grátis)
5. NÃO marca "Add a README", "Add .gitignore" nem licença
6. Clica **Create repository**

## Passo 2 — Subir os arquivos

Tem 2 caminhos. Escolhe um.

### Caminho A — pelo navegador (mais fácil, sem terminal)

1. Na página do repo, clica em **"uploading an existing file"**
2. Arrasta todos os arquivos da raiz de `pwa/`: index.html, manifest.json, service-worker.js, icon-192.png, icon-512.png
3. **Commit** → depois volta e cria a pasta `painel/` clicando em "Add file" → "Create new file", digita `painel/index.html` e cola o conteúdo do arquivo local
4. **Commit message:** `adiciona painel completo`

### Caminho B — pelo terminal (recomendado, pega tudo de uma vez)

```bash
cd "C:\Users\veron\OneDrive\Desktop\Claude\Amanda Assistente\pwa"
git init
git add .
git commit -m "pwa + painel completo"
git branch -M main
git remote add origin https://github.com/helloveronicaponce/amanda.git
git push -u origin main
```

Se o repo já existe (push rejeitado): `git push -u origin main --force`

## Passo 3 — Ativar GitHub Pages

1. No repo `amanda`, vai em **Settings** (aba lá em cima)
2. No menu da esquerda, **Pages**
3. **Source:** "Deploy from a branch"
4. **Branch:** `main` / pasta `/ (root)`
5. **Save**
6. Espera ~1 minuto, recarrega. Vai aparecer em cima:
   > Your site is live at https://helloveronicaponce.github.io/amanda/

## Passo 4 — Configurar Supabase (REDIRECT URL)

Pra o magic link de login funcionar:

1. Abre https://supabase.com/dashboard/project/qwlegnebejakwwuntyrd/auth/url-configuration
2. Em **Redirect URLs**, adiciona:
   ```
   https://helloveronicaponce.github.io/amanda/
   https://helloveronicaponce.github.io/amanda
   ```
3. Salva

## Passo 5 — Primeiro acesso e instalação no celular

### iPhone (Safari)
1. Abre Safari, vai em **https://helloveronicaponce.github.io/amanda/**
2. Digita seu e-mail (helloveronicaponce@gmail.com), clica "Enviar link de acesso"
3. Vai no app de e-mail, clica no link da Supabase, volta pro Safari
4. Já tá logada. Agora: botão **Compartilhar** (quadrado com seta pra cima) → role pra baixo → **Adicionar à Tela de Início**
5. Confirma. Vira ícone "Amanda" na tela inicial.

### Android (Chrome)
1. Abre Chrome, vai em **https://helloveronicaponce.github.io/amanda/**
2. Faz login igual acima
3. Menu ⋮ no canto → **Instalar app** (ou **Adicionar à tela inicial**)
4. Confirma. Vira app na gaveta.

## Como usar

1. Toca no ícone Amanda
2. Digita o que quiser anotar/pedir. Exemplos:
   - `paguei R$50 no mercado hoje`
   - `comprar fralda do Alberto`
   - `marca pediatra Gael quinta 14h`
   - `me lembra de levar autorização da excursão`
3. Se tiver foto (circular escolar, comprovante, foto de remédio): clica **📷 Foto** ou **🖼️ Galeria**
4. Toca **Enviar**

A Amanda desktop processa a cada 30 min (scheduled task `amanda-pwa-bridge`). Você vê o status no histórico do próprio app:
- 🟡 **pending** — esperando Amanda processar
- 🔵 **processing** — Amanda tá processando agora
- 🟢 **processed** — feito (com resumo do que ela fez)
- 🔴 **error** — deu erro (com a mensagem)

## Quero atualizar o app depois

Quando eu (Amanda) gerar uma nova versão dos arquivos, é só substituir no repo (pelo navegador ou `git push`) e o GitHub Pages atualiza automático. No celular, fecha e reabre o app — ele puxa a versão nova.

## Problemas comuns

**"Página em branco"** → o GitHub Pages demora 1-2 min na primeira vez. Espera e recarrega.

**"Magic link não chega"** → confere a redirect URL no Supabase (passo 4). Confere a caixa de spam.

**"Login não persiste"** → no Safari iOS, garante que "Bloquear todos os cookies" tá DESLIGADO em Ajustes → Safari.

**"Foto não envia"** → confere se você concedeu permissão de câmera/galeria pro Safari/Chrome quando ele perguntou.

**"Quero ver o que tá rolando no banco"** → me chama no desktop e fala "Amanda, mostra os pendentes do PWA" que eu rodo a query no Supabase.

## Pronto!

Qualquer coisa, me chama no desktop (`Amanda, ...`) e eu ajusto.
