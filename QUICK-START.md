# 🚀 Início Rápido - Git Ignore Security

> Guia de 5 minutos para começar

---

## ⚡ Setup Imediato

### 1️⃣ Copie o .gitignore (30 segundos)

```bash
# Copie o arquivo .gitignore para seu projeto
cp .gitignore /seu/projeto/.gitignore

# Commite imediatamente
cd /seu/projeto
git add .gitignore
git commit -m "Add comprehensive .gitignore for security"
git push
```

### 2️⃣ Crie o .env.example (1 minuto)

```bash
# Copie como base
cp .env.example /seu/projeto/.env.example

# Customize com suas variáveis
# Remova valores reais, deixe vazios ou com placeholders
nano /seu/projeto/.env.example

# Commite
git add .env.example
git commit -m "Add .env.example template"
git push
```

### 3️⃣ Crie seu .env Local (1 minuto)

```bash
# Crie o arquivo .env real
cd /seu/projeto
cp .env.example .env

# Preencha com valores REAIS
# MAS NÃO COMMITE
nano .env

# Confirme que está no .gitignore
git status  # .env não deve aparecer
```

### 4️⃣ Instale Pre-commit Hooks (2 minutos)

```bash
# Instalar
pip install pre-commit

# Criar arquivo de configuração
cat > /seu/projeto/.pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: detect-private-key
      - id: check-added-large-files
        args: ['--maxkb=1000']
EOF

# Instalar hooks
cd /seu/projeto
pre-commit install

# Rodar uma vez em todos os arquivos
pre-commit run --all-files
```

### 5️⃣ Configure GitHub Secret Scanning (1 minuto)

Se seu código está no GitHub:

```
Abra seu repositório
→ Settings
→ Code security & analysis
→ Enable "Secret scanning"
→ Enable "Dependabot alerts"
```

---

## ✅ Verificação Rápida

Use este checklist para confirmar:

```bash
# 1. Verifique .gitignore
cat /seu/projeto/.gitignore | grep -E "\.env|\.pem|\.key|\.tfvars"

# 2. Verifique se .env está ignorado
cd /seu/projeto
git status | grep ".env"  # Não deve aparecer

# 3. Verifique pré-commit
pre-commit run --all-files

# 4. Verifique histórico
git log -p | grep -i "password\|api_key\|secret" | head

# 5. Teste commit
echo "test" > testfile.txt
git add testfile.txt
git commit -m "Test pre-commit"  # Deve rodar hooks
```

---

## 🔥 Padrões Essenciais

Se você só tiver tempo para 3 minutos, adicione ISTO ao `.gitignore`:

```gitignore
# CRÍTICO - Sempre ignore
.env
.env.local
.env.*.local
*.tfvars
*.tfvars.json
.terraform/
terraform.tfstate*

# CHAVES
*.pem
*.key
*.crt
id_rsa
id_rsa.pub

# CLOUD
.aws/
.gcp/
.azure/
kubeconfig

# DATABASE
*.db
dump.sql
database.yml

# LOGS
*.log
logs/

# NODE
node_modules/

# PYTHON
venv/
env/

# IDE
.vscode/
.idea/
```

---

## 🛠️ Ferramentas Essenciais (3 ferramentas)

Se você só instalar 3:

### 1. Git-Secrets
Previne commit de secrets:
```bash
# macOS
brew install git-secrets
git secrets --install
git secrets --register-aws

# Linux
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets && make install
```

### 2. TruffleHog
Encontra secrets já commitados:
```bash
pip install truffleHog
truffleHog filesystem /seu/projeto
```

### 3. Gitleaks
Detecta secrets em CI/CD:
```bash
brew install gitleaks
# Ou em GitHub Actions:
# name: Gitleaks
# uses: gitleaks/gitleaks-action@v2
```

---

## 🚨 Encontrou Secret Commitado?

**EXECUTE IMEDIATAMENTE:**

```bash
# 1. REVOGUE IMEDIATAMENTE
# - Regenere API keys
# - Mude senhas de DB
# - Revoke tokens GitHub/AWS

# 2. Remova do histórico
pip install git-filter-repo
git filter-repo --invert-paths --path .env

# 3. Force push (CUIDADO!)
git push origin --force-with-lease

# 4. Notifique equipe
# "Secret foi exposto, favor fazer pull --rebase"
```

---

## 📚 Próximas Leituras

### 5 Min (Fez)
- ✅ Copiou `.gitignore`
- ✅ Criou `.env.example`
- ✅ Instalou pre-commit

### 15 Min (Faça Hoje)
- [ ] Leia [README.md](./README.md) - Por Que Isso Importa
- [ ] Use [SECURITY-CHECKLIST.md](./SECURITY-CHECKLIST.md) - Pré-Commit

### 1 Hora (Esta Semana)
- [ ] Leia [git-list-ignore.md](./git-list-ignore.md) - Padrões completos
- [ ] Instale 1 ferramenta extra

### 3 Horas (Este Mês)
- [ ] Estude [SECURITY-TOOLS.md](./SECURITY-TOOLS.md) - Ferramentas
- [ ] Implemente monitoramento contínuo

---

## 🎯 Metas por Semana

### Semana 1
- [ ] `.gitignore` commitado
- [ ] `.env.example` criado
- [ ] `pre-commit` instalado
- [ ] Nenhum secret detectado

### Semana 2
- [ ] GitHub Secret Scanning ativado
- [ ] Equipe notificada
- [ ] Documentação compartilhada
- [ ] 1 ferramenta implementada

### Semana 3-4
- [ ] Audit concluído
- [ ] Issues resolvidas
- [ ] Monitoramento ativo
- [ ] Equipe treinada

---

## 🤔 FAQ Rápido

**P: Por que .env não deve ser commitado?**
R: Contém senhas, chaves API e credenciais reais.

**P: E se eu usar variáveis de ambiente?**
R: Ainda assim, use `.env.example` como template, não `.env`.

**P: O que fazer com `.env.example`?**
R: Commite SEM valores reais, apenas variáveis vazias ou fake.

**P: Pre-commit hooks vai bloquear meus commits?**
R: Sim, de secrets! Isso é bom.

**P: Posso usar .env em produção?**
R: NÃO. Use gerenciadores de secrets (AWS Secrets Manager, Vault).

**P: Quanto tempo leva implementar?**
R: 5 minutos de setup + 1 hora para tudo estar pronto.

**P: E se foi commitado antes?**
R: Imediatamente revogue e remova do histórico (veja seção acima).

---

## 📞 Suporte Rápido

### Erro: "git-secrets" não encontrado
```bash
# macOS
brew install git-secrets

# Linux
sudo apt-get install git-secrets
```

### Erro: ".env" não está sendo ignorado
```bash
# Remova do cache
git rm --cached .env

# Adicione ao .gitignore
echo ".env" >> .gitignore

# Commite
git add .gitignore
git commit -m "Update .gitignore"
```

### Erro: "pre-commit not found"
```bash
pip install pre-commit
pre-commit install
```

---

## 🎓 Depois Disso

Uma vez que está setup:

1. **Mantenha atualizado**: Review `.gitignore` mensalmente
2. **Eduque a equipe**: Compartilhe [README.md](./README.md)
3. **Automatize**: Configure GitHub Actions para secret scanning
4. **Monitore**: Use [SECURITY-TOOLS.md](./SECURITY-TOOLS.md)
5. **Audite**: Use [SECURITY-CHECKLIST.md](./SECURITY-CHECKLIST.md) quarterly

---

## 🔗 Documentação Completa

- 📘 [Índice](./INDEX.md) - Mapa de documentação
- 📖 [README.md](./README.md) - Visão completa
- 📋 [git-list-ignore.md](./git-list-ignore.md) - Padrões detalhados
- ✅ [SECURITY-CHECKLIST.md](./SECURITY-CHECKLIST.md) - Auditorias
- 🛠️ [SECURITY-TOOLS.md](./SECURITY-TOOLS.md) - Ferramentas

---

**⏱️ Você tem 5 minutos? [Faça o setup](#️-setup-imediato)**

**Tempo total para produção:** 1 hora

**Benefício:** Protege 99% dos vazamentos de secrets

---

> 🔐 **Primeiro step é sempre o melhor step** - Comece hoje mesmo!
