---
title: Segurança - Credenciais Expostas no GitHub
date: 2026-01-27
status: urgent
tags: [segurança, credenciais, git, urgente]
---

# ⚠️ AÇÃO URGENTE: Credenciais Expostas no GitHub

## Situação

O arquivo `.claude/.credentials.json` contendo tokens de acesso da API Claude foi commitado e pode estar exposto no GitHub.

## Ações Imediatas Necessárias

### 1. Invalidar os Tokens Expostos (CRÍTICO)

**Os tokens que foram commitados precisam ser invalidados imediatamente:**

1. Acesse sua conta Claude/Anthropic
2. Vá para configurações de segurança/API
3. Revogue os tokens expostos:
   - `accessToken`: `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`
   - `refreshToken`: `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`
4. Gere novos tokens após revogar os antigos

### 2. Remover do Histórico do Git (Se o Repositório é Público)

Se o repositório for **público** no GitHub, o arquivo ainda estará visível no histórico mesmo após remover do commit atual.

**Opções:**

#### Opção A: Usar git-filter-repo (Recomendado)
```bash
# Instalar git-filter-repo se necessário
pip install git-filter-repo

# Remover o arquivo de todo o histórico
git filter-repo --path .claude/.credentials.json --invert-paths

# Forçar push (CUIDADO: isso reescreve o histórico)
git push origin --force --all
```

#### Opção B: Usar BFG Repo-Cleaner
```bash
# Baixar BFG
# https://rtyley.github.io/bfg-repo-cleaner/

# Remover o arquivo
bfg --delete-files .claude/.credentials.json

# Limpar e fazer push
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin --force --all
```

#### Opção C: Se o repositório é privado
- Apenas fazer o commit removendo o arquivo pode ser suficiente
- Mas ainda é recomendado invalidar os tokens

### 3. Commit das Correções

Após invalidar os tokens, fazer commit das mudanças:

```bash
git add .gitignore
git commit -m "security: remove credentials file and add to .gitignore"
git push
```

### 4. Verificar se Há Outros Arquivos Sensíveis

```bash
# Procurar por possíveis arquivos de credenciais
find . -name "*credentials*" -o -name "*secret*" -o -name "*.env" | grep -v node_modules
```

## Prevenção Futura

### ✅ Já Implementado

- `.gitignore` criado com regras para credenciais
- Arquivo removido do rastreamento do git

### 📋 Checklist de Segurança

- [ ] Invalidar tokens expostos
- [ ] Gerar novos tokens
- [ ] Fazer commit das correções
- [ ] (Se público) Limpar histórico do git
- [ ] Verificar outros arquivos sensíveis
- [ ] Configurar git-secrets ou similar para prevenir commits futuros

## Ferramentas Recomendadas

### git-secrets (Prevenção)
```bash
# Instalar
brew install git-secrets  # macOS
# ou
git clone https://github.com/awslabs/git-secrets.git

# Configurar
cd git-secrets
sudo make install
git secrets --install
git secrets --register-aws
```

### TruffleHog (Detecção)
Ferramenta para escanear repositórios em busca de credenciais:
```bash
pip install truffleHog
trufflehog --regex --entropy=False .
```

## Notas

- O arquivo `.claude/.credentials.json` ainda existe localmente (isso é correto)
- Ele apenas não será mais rastreado pelo git
- Novos tokens serão gerados automaticamente quando necessário pelo sistema

---

**Status**: Aguardando invalidação dos tokens e commit das correções
**Prioridade**: 🔴 CRÍTICA - Fazer imediatamente
