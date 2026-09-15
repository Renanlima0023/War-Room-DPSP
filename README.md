# 🎛️ ROCC · Roll Out Control Center

> Sistema Kanban completo para gestão de rollout de lojas, com controle de PDVs, checklist operacional, dashboard de gestão e geração de RAT (Relatório de Atendimento Técnico).

![Status](https://img.shields.io/badge/status-em%20produção-success)
![Versão](https://img.shields.io/badge/versão-1.0.0-blue)
![Licença](https://img.shields.io/badge/licença-proprietária-red)

---

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Funcionalidades](#-funcionalidades)
- [Perfis de Acesso](#-perfis-de-acesso)
- [Fluxo de Trabalho](#-fluxo-de-trabalho)
- [Arquitetura](#-arquitetura)
- [Instalação](#-instalação)
- [Configuração](#-configuração)
- [Como Usar](#-como-usar)
- [Banco de Dados](#-banco-de-dados)
- [Solução de Problemas](#-solução-de-problemas)
- [Roadmap](#-roadmap)

---

## 🎯 Visão Geral

O **ROCC** é um sistema web (PWA) para gerenciar o processo completo de instalação e atualização de PDVs (Pontos de Venda) em lojas físicas. Centraliza:

- 📦 **Controle de lojas** em formato Kanban
- 👥 **Atribuição de responsáveis** (backoffice)
- 🔧 **Acompanhamento de PDVs** (antigo → atualizado)
- ✅ **Checklist operacional** compartilhado entre backoffice e técnico de campo
- 📊 **Dashboard** com métricas e carga de trabalho
- ✍️ **RAT digital** com assinatura do gerente e do técnico

---

## ✨ Funcionalidades

### 🗂️ Kanban de Lojas
- 5 colunas: **Agendada** → **Execução** → **Bloqueada** → **Revisão** → **Concluída**
- Arrastar e soltar (drag & drop) entre colunas
- Ordenação personalizada salva automaticamente
- Filtros por nome, responsável e UF
- Menu de contexto (editar, duplicar, remover)

### 👥 Gestão de Usuários
- 5 perfis: Admin, Gerente, Técnico, Visualizador e Monitoramento
- Atribuição de responsáveis por loja
- Reset de senha
- Edição e remoção

### 🔌 Controle de PDVs
- Cadastro por loja com quantidade automática
- Comparativo **PDV Antigo → PDV Atualizado**
- Número de série editável
- 4 status: Pendente, Em Andamento, Concluído, Bloqueado
- Barra de progresso circular

### ✅ Checklist Operacional
- **Backoffice** cria os itens no card
- **Técnico** (via link) visualiza e marca/desmarca
- Progresso em tempo real (contador + barra)
- Sincronização automática com banco

### 📊 Dashboard de Gestão
- 5 cards de resumo por status
- Gráfico de barras: **Carga de Trabalho por Responsável**
- Gráfico doughnut: **Distribuição por Status**
- Detalhamento numérico por responsável
- Resumo geral (total, média, responsáveis ativos)

### ✍️ RAT - Relatório de Atendimento Técnico
- Assinatura digital via canvas (desenho ou digitação)
- Assinatura do **Gerente** (com matrícula)
- Assinatura do **Técnico**
- Geração de PDF para impressão
- Logo e branding Engemon IT

### 🌐 Compartilhamento por Link
- Gera link temporário para o técnico acessar **apenas uma loja**
- Sem necessidade de login
- Envio via WhatsApp integrado

### 🔔 Extras
- Notificações em tempo real (Supabase Realtime)
- Modo tela cheia (mais cards visíveis)
- Exportação para Excel
- Tema personalizável (color picker)
- PWA instalável
- Modo somente leitura (Monitoramento)

---

## 👤 Perfis de Acesso

| Perfil | Criar Loja | Editar | Remover | Ver Dashboard | Gerar Link | Assinar RAT |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| **Admin** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Gerente** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Técnico** | ⚠️ | ❌ | ❌ | ✅ | ❌ | ✅ |
| **Visualizador** | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| **Monitoramento** | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |

> ⚠️ O **Monitoramento (Service Desk)** tem acesso **somente leitura** — ideal para acompanhamento sem risco de alterações acidentais.

---

## 🔄 Fluxo de Trabalho

### 📌 Visão Geral do Processo

```
┌─────────────────────────────────────────────────────────────────┐
│                      FLUXO COMPLETO DO ROCC                     │
└─────────────────────────────────────────────────────────────────┘

  [ BACKOFFICE ]                    [ TÉCNICO DE CAMPO ]
        │                                    │
        │ 1. Cria loja no Kanban             │
        │    (Número, Cidade, UF)            │
        ├─────────────────────────────────►  │
        │                                    │
        │ 2. Atribui responsável             │
        │    (técnico backoffice)            │
        ├─────────────────────────────────►  │
        │                                    │
        │ 3. Cadastra PDVs                   │
        │    (automático por quantidade)     │
        ├─────────────────────────────────►  │
        │                                    │
        │ 4. Cria checklist operacional      │
        │    (ex: Configurar rede,           │
        │     Instalar It Lean...)           │
        ├─────────────────────────────────►  │
        │                                    │
        │ 5. Gera link único                 │
        │    e envia via WhatsApp            │
        ├───────── LINK ─────────────────────┤
        │                                    │
        │                          6. Abre link
        │                             Digita o nome
        │                                    │
        │                          7. Vê checklist
        │                             (do backoffice)
        │                                    │
        │                          8. Marca itens
        │                             conforme executa
        │                                    │
        │                          9. Assina + Gerente assina
        │                                    │
        │◄──────── SINCRONIZAÇÃO ────────────┤
        │                                    │
        │ 10. Vê progresso atualizado        │
        │     no card e dashboard            │
        ▼                                    ▼
```

---

### 🔹 Passo 1 — Backoffice cria a loja

**Tela:** Kanban → Botão **"+ Adicionar"** em qualquer coluna

**Preenche:**
- Número da loja (ex: `3030`)
- Cidade e UF
- Responsável (qualquer usuário ativo, exceto Monitoramento/Visualizador)
- Quantidade de PDVs (padrão: 3)
- Status inicial: **Agendada**
- Data agendada
- Regional (opcional)

**Resultado:** Card aparece na coluna "Agendadas" com avatar do responsável.

---

### 🔹 Passo 2 — Backoffice cadastra PDVs

**Tela:** Clicar no card → aba **"PDVs"**

**Ações:**
- Ver lista de PDVs criados automaticamente (`PDV 01`, `PDV 02`...)
- Editar nome antigo (clique no badge roxo)
- Editar nome atualizado (clique no badge laranja)
- Editar número de série (campo de texto)
- Alterar status individual ou em massa
- Adicionar novos PDVs

**Status disponíveis:**
| Ícone | Status | Cor |
|---|---|---|
| ⏳ | Pendente | Amarelo |
| 🔄 | Em Andamento | Azul (pulsante) |
| ✅ | Concluído | Verde |
| ⛔ | Bloqueado | Rosa |

---

### 🔹 Passo 3 — Backoffice cria o Checklist

**Tela:** Card → aba **"Checklist"** → botão **"+ Adicionar item"**

**Exemplos de itens:**
- Configurar rede dos PDVs
- Instalar Linux nos terminais
- Instalar It Lean
- Realizar venda teste
- Assinar termo de aceite

**Controle total:** o backoffice pode editar texto, remover itens e reordenar.

---

### 🔹 Passo 4 — Backoffice gera o link para o técnico

**Tela:** Card → aba **"Link"**

**Passos:**
1. Digite o nome do técnico (ex: `João Silva`)
2. Clique em **"Gerar Link para Técnico"**
3. Copie o link ou clique em **"Enviar WhatsApp"**

**O link contém:**
- Token de acesso temporário (24h)
- ID da loja (acesso restrito a essa única loja)
- Sem necessidade de login

---

### 🔹 Passo 5 — Técnico acessa pelo link

**Tela:** Página de identificação → digita o nome completo → **"Acessar Loja"**

**O técnico vê:**
- 📋 Nome da loja, cidade, UF e ID
- 👤 Seu próprio nome como técnico responsável
- 🔌 Lista de PDVs (com status e nº de série)
- ✅ **Checklist** com os itens criados pelo backoffice
- 📊 Progresso visual (ex: `0/5`, barra verde)
- 📝 Observações da loja
- ✍️ Botão **"Assinar e Gerar RAT"**

---

### 🔹 Passo 6 — Técnico marca o checklist

**Ao marcar um item:**
- ✅ Notificação: `"Configurar rede dos PDVs" concluído!`
- ✅ Contador atualiza (ex: `1/5`)
- ✅ Barra de progresso sobe
- ✅ Salvamento automático no Supabase

**Ao marcar todos:**
- 🎉 Mensagem: `"Todos os itens do checklist foram concluídos!"`

**Sincronização:** o backoffice vê as marcações em tempo real (ou após F5).

---

### 🔹 Passo 7 — Assinatura e RAT

**Tela:** Botão **"Assinar e Gerar RAT"**

**O técnico preenche:**
- Nome completo do **Gerente da loja**
- Matrícula do gerente

**Duas assinaturas:**

1. **Gerente** (assina na tela do tablet/PC):
   - Nome
   - Matrícula
   - Assinatura via mouse/dedo (canvas) **ou** digitada

2. **Técnico**:
   - Nome (já pré-preenchido)
   - Assinatura via mouse/dedo **ou** digitada

**Ao clicar em "Gerar RAT em PDF":**
- Abre uma nova janela com o documento
- Basta clicar em **"Imprimir"** → **"Salvar como PDF"**

---

### 🔹 Passo 8 — Backoffice acompanha

**No Kanban:** arrasta o card de "Execução" → "Concluída" quando finalizar

**No Dashboard:**
- Vê a carga de trabalho por responsável
- Distribuição por status
- Métricas do dia
- Produtividade

---

## 🏗️ Arquitetura

```
┌──────────────────────────────────────────────────────────┐
│                      FRONTEND                            │
│                                                          │
│  ┌────────────────────────────────────────────────┐     │
│  │         index.html (SPA - Single Page)         │     │
│  │                                                │     │
│  │  • TailwindCSS (via CDN)                       │     │
│  │  • Font Awesome (ícones)                       │     │
│  │  • Chart.js (gráficos do dashboard)            │     │
│  │  • XLSX (exportação Excel)                     │     │
│  │  • Supabase JS SDK                             │     │
│  └────────────────────────────────────────────────┘     │
│                          │                               │
└──────────────────────────┼───────────────────────────────┘
                           │
                           │ HTTPS
                           ▼
┌──────────────────────────────────────────────────────────┐
│                     BACKEND (BaaS)                       │
│                                                          │
│  ┌────────────────────────────────────────────────┐     │
│  │              SUPABASE                          │     │
│  │                                                │     │
│  │  📊 PostgreSQL Database                        │     │
│  │     ├─ Tabela: usuarios                        │     │
│  │     ├─ Tabela: lojas                           │     │
│  │     ├─ Tabela: pdvs                            │     │
│  │     └─ Tabela: responsaveis                    │     │
│  │                                                │     │
│  │  🔄 Realtime (WebSocket)                       │     │
│  │     └─ postgres_changes em "lojas"             │     │
│  │                                                │     │
│  │  🔐 Storage (não utilizado ainda)              │     │
│  └────────────────────────────────────────────────┘     │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### Stack Técnica

| Camada | Tecnologia |
|---|---|
| **Frontend** | HTML5 + JavaScript Vanilla |
| **Estilo** | TailwindCSS (CDN) |
| **Ícones** | Font Awesome 6 |
| **Gráficos** | Chart.js 4 |
| **Planilhas** | SheetJS (XLSX) 0.18 |
| **Backend** | Supabase (PostgreSQL + Realtime) |
| **Hospedagem** | Vercel |

---

## 🚀 Instalação

### Pré-requisitos

- Navegador moderno (Chrome, Edge, Firefox)
- Conta no [Supabase](https://supabase.com) (gratuita)
- Conta no [Vercel](https://vercel.com) (gratuita) — para deploy

### Passo 1 — Clonar o repositório

```bash
git clone https://github.com/seu-usuario/rocc.git
cd rocc
```

### Passo 2 — Configurar o Supabase

1. Crie um projeto no Supabase
2. Vá em **SQL Editor** e execute o script de criação das tabelas (ver seção [Banco de Dados](#-banco-de-dados))
3. Copie a **URL** e a **Publishable Key** em:
   **Settings → API → Project URL** e **Publishable Key**

### Passo 3 — Configurar credenciais no HTML

Abra o `index.html` e localize:

```javascript
const SUPABASE_URL = 'https://SEU-PROJETO.supabase.co';
const SUPABASE_KEY = 'sb_publishable_SUA_CHAVE_AQUI';
```

Substitua pelos valores do seu projeto.

### Passo 4 — Rodar localmente

Basta abrir o `index.html` no navegador. **Ou** rodar um servidor local:

```bash
# Python 3
python -m http.server 8000

# Node.js (com npx)
npx serve
```

Acesse: `http://localhost:8000`

### Passo 5 — Deploy no Vercel

**Via GitHub (recomendado):**
1. Suba o código para o GitHub
2. Vá em [vercel.com/new](https://vercel.com/new)
3. Importe o repositório
4. Deploy automático a cada push

**Via CLI:**
```bash
npm i -g vercel
vercel --prod
```

> ⚠️ **Após cada deploy**, faça **Redeploy sem cache** para garantir que as mudanças apareçam.

---

## ⚙️ Configuração

### Login padrão

Na primeira execução, se a tabela `usuarios` estiver vazia, o sistema cria um usuário padrão:

| Campo | Valor |
|---|---|
| **E-mail** | `admin@rocc.com` |


> 🔒 **Importante:** troque essas credenciais após o primeiro login.

### Variáveis de ambiente

Como o sistema é frontend puro, as credenciais do Supabase estão no HTML. Para maior segurança em produção, considere migrar para um backend intermediário.

---

## 📖 Como Usar

### 🔐 Login

1. Acesse a URL do sistema
2. Insira e-mail e senha
3. Clique em **"Entrar"**

### ➕ Criar uma loja

1. Clique em **"+ Adicionar"** em qualquer coluna
2. Preencha os campos obrigatórios (marcados com `*`)
3. Clique em **"Criar Loja"**

### 🖱️ Mover loja entre colunas

- **Desktop:** arraste o card
- **Mobile:** (a implementar)

### ✏️ Editar loja

1. Passe o mouse sobre o card
2. Clique nos **3 pontinhos** no canto superior
3. Escolha **"Editar"**
4. Modifique os campos e clique em **"Salvar"**

### 🔗 Gerar link para técnico

1. Abra o card
2. Vá na aba **"Link"**
3. Digite o nome do técnico
4. Clique em **"Gerar Link"**
5. Copie ou envie via WhatsApp

### 📊 Ver dashboard

1. No topo, clique em **"Dashboard"**
2. Veja os gráficos e métricas
3. Use o filtro para ver um responsável específico
4. Clique em **"Fechar Dashboard"** para voltar

### 📤 Exportar dados

Clique em **"Exportar Excel"** no topo → baixa um arquivo `.xlsx` com todas as lojas.

### 🎨 Personalizar cores

**Apenas para Admin:**
1. Clique em **"Configurações"** no topo
2. Ajuste as cores com os seletores
3. Clique em **"Salvar"**

---

## 🗄️ Banco de Dados

### Script SQL de criação

Execute no **SQL Editor** do Supabase:

```sql
-- ============================================================
-- TABELA: usuarios
-- ============================================================
CREATE TABLE IF NOT EXISTS usuarios (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nome TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    senha TEXT NOT NULL,
    role TEXT NOT NULL DEFAULT 'tecnico',
    ativo BOOLEAN DEFAULT TRUE,
    criado_em TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================================
-- TABELA: lojas
-- ============================================================
CREATE TABLE IF NOT EXISTS lojas (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nome TEXT NOT NULL,
    codigo TEXT UNIQUE,
    responsavel_id UUID REFERENCES usuarios(id) ON DELETE SET NULL,
    responsavel_nome TEXT,
    status TEXT DEFAULT 'agendada',
    regional TEXT,
    cidade TEXT,
    uf TEXT,
    pdvs INTEGER DEFAULT 0,
    comentarios INTEGER DEFAULT 0,
    data_agendada DATE,
    descricao TEXT,
    ordem INTEGER DEFAULT 0,
    checklist JSONB DEFAULT '[]'::jsonb,
    prioridade TEXT,
    subtipo TEXT,
    criado_em TIMESTAMPTZ DEFAULT NOW(),
    atualizado_em TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================================
-- TABELA: pdvs
-- ============================================================
CREATE TABLE IF NOT EXISTS pdvs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    loja_id UUID REFERENCES lojas(id) ON DELETE CASCADE,
    codigo TEXT NOT NULL,
    nome_antigo TEXT,
    nome_atualizado TEXT,
    numero_serie TEXT,
    status TEXT DEFAULT 'pendente',
    observacao TEXT,
    criado_em TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================================
-- TABELA: responsaveis (opcional - tabela auxiliar)
-- ============================================================
CREATE TABLE IF NOT EXISTS responsaveis (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    nome TEXT NOT NULL,
    criado_em TIMESTAMPTZ DEFAULT NOW()
);

-- ============================================================
-- RLS - Row Level Security
-- ============================================================
ALTER TABLE usuarios ENABLE ROW LEVEL SECURITY;
ALTER TABLE lojas ENABLE ROW LEVEL SECURITY;
ALTER TABLE pdvs ENABLE ROW LEVEL SECURITY;
ALTER TABLE responsaveis ENABLE ROW LEVEL SECURITY;

-- Policies liberadas para "anon" (frontend sem login)
CREATE POLICY "anon_all_usuarios" ON usuarios FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "anon_all_lojas" ON lojas FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "anon_all_pdvs" ON pdvs FOR ALL TO anon USING (true) WITH CHECK (true);
CREATE POLICY "anon_all_responsaveis" ON responsaveis FOR ALL TO anon USING (true) WITH CHECK (true);

-- ============================================================
-- USUÁRIO PADRÃO (Admin)
-- ============================================================
INSERT INTO usuarios (nome, email, senha, role, ativo)
VALUES ('Administrador', 'admin@rocc.com', 'admin123', 'admin', true)
ON CONFLICT (email) DO NOTHING;
```

### Estrutura da coluna `checklist`

```json
[
  {
    "id": "item-1234567890",
    "texto": "Configurar rede dos PDVs",
    "concluido": false
  },
  {
    "id": "item-1234567891",
    "texto": "Instalar It Lean",
    "concluido": true
  }
]
```

---

## 🐛 Solução de Problemas

### ❌ Não aparece nenhum nome no select "Atribuir Responsável"

**Causa:** Todos os usuários têm role `monitoramento` ou `visualizador`, ou email vazio.

**Solução:**
1. Verifique no Supabase: `SELECT nome, email, role FROM usuarios;`
2. Cadastre pelo menos 1 usuário com role `admin`, `gerente` ou `tecnico`
3. Confirme que o campo `email` está preenchido

---

### ❌ Alterações não aparecem no Vercel

**Causa:** Cache do Vercel ou do navegador.

**Solução:**
1. No Vercel → **Deployments** → último deploy → **Redeploy** (desmarque "Use cache")
2. No navegador: **Ctrl + Shift + R** (hard refresh)
3. Ou abra em aba anônima

---

### ❌ Erro: `WebSocket connection to ... failed`

**Causa:** Supabase Realtime bloqueado.

**Impacto:** Apenas notificações em tempo real (não afeta o funcionamento geral).

**Solução:**
1. Supabase → **Database → Replication** → habilitar `lojas`
2. Verificar se a rede/firewall permite WebSocket

---

### ❌ Erro: `new row violates row-level security policy`

**Causa:** Falta de policies RLS no Supabase.

**Solução:** Execute os `CREATE POLICY` do script SQL acima.

---

### ❌ Cores personalizadas não salvam

**Causa:** `localStorage` bloqueado.

**Solução:** Verificar se o navegador está em modo anônimo ou com cookies bloqueados.

---

### ❌ Link do técnico não abre

**Causas possíveis:**
- Link expirado (válido por 24h)
- Query string removida no redirecionamento do Vercel

**Solução:** Gerar um novo link no card.

---

## 🎯 Roadmap

### ✅ Concluído (v1.0)
- [x] Kanban com 5 colunas
- [x] Drag & drop com persistência
- [x] Gestão de usuários (5 perfis)
- [x] Controle de PDVs
- [x] Checklist compartilhado (backoffice ↔ técnico)
- [x] Dashboard com métricas e gráficos
- [x] RAT com assinatura digital
- [x] Geração de link temporário
- [x] Exportação para Excel
- [x] Tema personalizável
- [x] PWA instalável

### 🚧 Em andamento (v1.1)
- [ ] Correção do Realtime (WebSocket)
- [ ] Melhorias de acessibilidade mobile
- [ ] Histórico de alterações (auditoria)

### 🔮 Planejado (v2.0)
- [ ] Upload de evidências fotográficas
- [ ] Assinatura em massa (múltiplas lojas)
- [ ] Filtro por progresso do checklist no Kanban
- [ ] Notificações push
- [ ] Modo offline (Service Worker)
- [ ] Relatórios avançados (PDF/CSV)
- [ ] Integração com WhatsApp Business API

---

## 👥 Contribuindo

Este é um projeto proprietário da **Renan Lima**. Para sugestões, entre em contato com o time de desenvolvimento.

---

## 📄 Licença

Proprietário · **Renan Lima - Rinfotecsolucoes** · Todos os direitos reservados © 2026

---

## 📞 Suporte

- **E-mail:** renan.lima@engemon.com.br
- **Documentação interna:** [Wiki interna]
- **Issues:** [GitHub Issues](https://github.com/seu-usuario/rocc/issues)

---

<div align="center">

**Desenvolvido por Renan Lima - Rinfotecsolucoes**

[⬆ Voltar ao topo](#-rocc--roll-out-control-center)

</div>
