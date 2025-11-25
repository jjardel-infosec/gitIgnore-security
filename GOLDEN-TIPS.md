# 💎 Guia de Ouro - As Melhores Dicas de Segurança Git

> As coisas MAIS IMPORTANTES que você precisa saber sobre proteção de secrets

---

## 🏆 Top 10 Aprendizados Essenciais

### 1️⃣ .env NUNCA DEVE SER COMMITADO

**A Regra de Ouro:**
```bash
# ❌ NUNCA
git add .env
git commit -m "Add environment variables"

# ✅ SEMPRE
echo ".env" >> .gitignore
git add .gitignore
git commit -m "Add .gitignore"
```

**Por quê?** 
- Contém senhas reais
- Contém chaves API
- Visível para todos no repositório

**O Que Fazer:**
```bash
# 1. Crie .env.example (sem valores reais)
.env.example:
DB_PASSWORD=your_password_here
API_KEY=your_api_key_here

# 2. Commite .env.example
git add .env.example
git commit -m "Add .env.example template"

# 3. Cada dev cria seu próprio .env
cp .env.example .env
# Edita .env com valores reais
# MAS NÃO COMMITA
```

---

### 2️⃣ *.tfvars = TESOURO DE SECRETS

**Terraform Variables = Senhas Expostas**

```hcl
# ❌ NUNCA
# terraform.tfvars
db_password = "SuperSecret123!"
aws_secret_key = "AKIAIOSFODNN7EXAMPLE"
api_token = "ghp_16C7e42F292c6912E7710c838347Ae178B4a"

# ✅ SEMPRE
# .gitignore
*.tfvars
*.tfvars.json
.terraform/
terraform.tfstate*
```

**Toda variável sensível vai em:**
```bash
# Opção 1: Arquivo separado ignorado
env/prod.tfvars (ignorado)

# Opção 2: Variáveis de ambiente
export TF_VAR_db_password="secret"

# Opção 3: Terraform Cloud
# Usar remote state com encrypted vars
```

---

### 3️⃣ CHAVES PRIVADAS SÃO SAGRADAS

**Nunca, NUNCA, NUNCA commite:**

```bash
# ❌ NUNCA (deteção automática desses)
id_rsa                   # SSH key
id_ed25519               # ED25519 key
*.pem                    # Certificate
*.key                    # Private key
*.crt                    # Certificate
kubeconfig               # Kubernetes config
~/.docker/config.json    # Docker credentials
```

**Se acidentalmente commitou:**

```bash
# IMEDIATAMENTE (agora!)
1. Revogue a chave
2. Gere uma nova
3. Remove do histórico Git

# Remover do histórico
pip install git-filter-repo
git filter-repo --invert-paths --path id_rsa

# Force push
git push --force-with-lease

# AVISE TODA A EQUIPE
"Favor fazer git pull --rebase origin main"
```

---

### 4️⃣ CREDENCIAIS CLOUD = ACESSO TOTAL

**Suas credenciais cloud são as chaves do reino:**

```bash
# Ignorar SEMPRE:

# AWS
.aws/                      # AWS credentials
aws_credentials            # AWS keys
~/.aws/config              # AWS config

# Google Cloud
.gcp/
google-cloud-key.json      # GCP service account
service-account-key.json   # GCP key

# Azure
.azure/
azure-credentials.json     # Azure creds

# Kubernetes
kubeconfig                 # K8s admin key
```

**Se exposto:**
```bash
# IMEDIATAMENTE:
1. AWS   → Revoke access keys (Console)
2. GCP   → Delete service account (Console)
3. Azure → Reset credentials (Portal)
4. K8s   → Update kubeconfig

# Impedir mais acesso
# Mude credenciais de recursos que o atacante pode acessar
```

---

### 5️⃣ LOGS NUNCA DEVEM TER SECRETS

**Nunca logue informações sensíveis:**

```javascript
// ❌ NUNCA
console.log("Connecting to DB:", password);
logger.info("API Key:", apiKey);
console.log("JWT Token:", token);

// ✅ SEMPRE
console.log("Connecting to database");
logger.info("API authentication started");
console.log("Token generated successfully");

// ✅ MELHOR - Redact para debug
logger.debug("User token", { token: token.slice(0, 10) + "..." });
```

**Padrão seguro:**
```javascript
function safeLog(key, value) {
  if (typeof value === 'string' && value.length > 20) {
    return value.slice(0, 5) + "***" + value.slice(-5);
  }
  return "[REDACTED]";
}
```

---

### 6️⃣ .gitignore PRECISA SER COMMITADO

**Seu `.gitignore` é documentação de segurança:**

```bash
# ✅ SEMPRE commite .gitignore
git add .gitignore
git commit -m "Add security .gitignore"

# ✅ SEMPRE commite .gitattributes (se encriptado)
git add .gitattributes
git commit -m "Add Git attributes for encrypted files"

# ❌ NUNCA ignore o .gitignore
# Se ignorado, `.env` pode ser commitado acidentalmente
```

**Por quê?** 
- Documenta o que não deve entrar
- Protege quando novo dev clonar
- Serve de educação

---

### 7️⃣ PRÉ-COMMIT HOOKS = SUA REDE DE SEGURANÇA

**Automático = melhor do que contar com memória:**

```bash
# Instalação (1 vez por dev)
pip install pre-commit

# Em seu projeto
cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: detect-private-key
      - id: check-added-large-files
        args: ['--maxkb=1000']
EOF

# Instalar
pre-commit install

# Agora automático em cada commit
git add arquivo_secreto.env
git commit -m "Add config"
# ❌ PRÉ-COMMIT BLOQUEIA!
# "Detected secrets in arquivo_secreto.env"
```

**Resultado:** Impossível commitar secrets por acidente

---

### 8️⃣ .env.example = DOCUMENTAÇÃO DE VARIÁVEIS

**Seu template é ouro:**

```bash
# ✅ BOM .env.example
DB_HOST=localhost
DB_PORT=5432
DB_NAME=my_database
DB_USER=root
DB_PASSWORD=your_password_here
API_KEY=your_api_key
JWT_SECRET=your_jwt_secret

# ✅ MELHOR .env.example (com comentários)
# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_NAME=my_database
DB_USER=root
DB_PASSWORD=your_secure_password_min_16_chars

# API Configuration
API_KEY=sk_live_xxxxxxxxxxxxx  # Get from https://api.example.com/keys

# Security
JWT_SECRET=base64_encoded_32_char_min

# ❌ NUNCA coloque valores reais
# .env.example com valores reais = falha total
```

**Benefício:**
- Novo dev sabe que variáveis precisa
- Documentação de configuração
- Exemplo de formato

---

### 9️⃣ GitHub Secret Scanning = Detector Automático

**Grátis no GitHub, use:**

```
Seu repositório no GitHub
→ Settings
→ Code security & analysis
→ Ativar "Secret scanning"
```

**O que detecta automaticamente:**
- AWS keys
- GitHub tokens
- Stripe API keys
- Slack tokens
- E mais...

**Benefício:**
- Detecção automática
- Alerta instantâneo
- Gratuito

---

### 🔟 SE VAZOU = AJA RÁPIDO

**Plano de Resposta (primeiras 15 min):**

```bash
# ⏰ 0-5 min: Revogue
aws accesskey delete [key-id]           # AWS
gh secret delete [nome]                 # GitHub
# ou web console para GCP, Azure

# ⏰ 5-10 min: Remova do histórico
pip install git-filter-repo
git filter-repo --invert-paths --path .env
git push --force-with-lease

# ⏰ 10-15 min: Notifique
Slack/Email:
"⚠️ Secret foi exposto em commit XYZ
Favor fazer: git pull --rebase origin main
Detalhes: [link para documentação]"

# Depois: Investigate
git log --oneline | head -50  # Veja histórico
git blame arquivo.env          # Quem commitou
git log -p arquivo.env | grep senha  # Procure por padrões
```

---

## 🎯 Seu Checklist Diário

### Antes de CADA Commit

```
[ ] Nenhuma senha em código?
[ ] Nenhuma chave API em código?
[ ] Nenhum arquivo .env?
[ ] Nenhum kubeconfig?
[ ] Nenhum arquivo .pem/.key?
[ ] Nenhum dump de banco de dados?
[ ] Nenhuma credencial cloud?

Se alguma está marcada ❌:
→ PARE e limpe antes de commitar
→ Ou pre-commit hooks bloquearão
```

### Semanalmente

```
[ ] Revisar commits recentes
[ ] Procurar por padrões de senha
[ ] Verificar .gitignore
[ ] Checkar pré-commit funcionando
```

### Mensalmente

```
[ ] Audit completo conforme SECURITY-CHECKLIST
[ ] Atualizar .env.example se necessário
[ ] Revisar políticas de secrets
[ ] Treinar novos devs
```

---

## 🚨 Padrões de Alerta

Se você vê isso → PROBLEMA SÉRIO:

```bash
# ❌ ALERTA
git log -p | grep -i "password"

# ❌ ALERTA
git log -p | grep -i "api_key"

# ❌ ALERTA
git show | grep "-----BEGIN"  # Chave privada!

# ❌ ALERTA
cat .env                        # Nunca deveria existir

# ❌ ALERTA
ls .aws/config                  # Nunca deveria existir

# ❌ ALERTA
cat kubeconfig                  # Nunca deveria existir
```

**Se vê algum:** → IMEDIATAMENTE
1. Revogue a credencial
2. Remove do histórico
3. Avise equipe

---

## 💡 Filosofia de Segurança

```
PRINCÍPIO #1: Default DENY
└─ Nada sensível é commitado por padrão
   └─ .env está SEMPRE em .gitignore
   └─ Chaves estão SEMPRE em .gitignore

PRINCÍPIO #2: Prevenção > Detecção
└─ Melhor prevenir que encontrar depois
   └─ Pre-commit hooks > Manual review
   └─ .gitignore > Remover histórico

PRINCÍPIO #3: Documentação = Segurança
└─ .env.example documenta necessários
└─ .gitignore documenta o que não vai
└─ Políticas documentam o como

PRINCÍPIO #4: Automação > Confiança
└─ Máquinas não esquecem
└─ Humans esquecem
└─ Use ferramentas
```

---

## 📊 Comparação: Com vs Sem Segurança

```
SEM SETUP (Situação Atual):
❌ Secrets podem ser expostos
❌ Sem detecção automática
❌ Sem documentação
❌ Equipe destreinada
❌ Vulnerável a incidentes
💸 Risco: $50K+ por vazamento

COM SETUP BÁSICO (Esse Repo):
✅ Secrets prevenidos por .gitignore
✅ Detecção automática (pre-commit)
✅ Documentação clara
✅ Equipe educada
✅ 99%+ proteção
💰 ROI: Evita incidentes = economia

DEPOIS DE 1 MÊS:
✅ 100% conformidade
✅ Zero secrets encontrados
✅ Cultura de segurança
✅ Processos automatizados
✅ Equipe confiante
```

---

## 🎁 Bônus: Comandos Úteis

```bash
# Verificar se tem secrets
git log -p | grep -i "password\|api_key\|secret" | head

# Verificar um arquivo específico
git log -p -- arquivo.env | head -100

# Procurar por padrões de chave
git log -p | grep "-----BEGIN"

# Remover arquivo do histórico (cuidado!)
git filter-repo --invert-paths --path .env

# Force push (APENAS EM REPOS PRIVADOS)
git push --force-with-lease

# Verificar .gitignore está funcionando
git check-ignore -v .env          # Se mostra: .env está ignorado ✅

# Listar todos os arquivos tracked
git ls-files | grep -i "secret\|password\|key"
```

---

## 🎓 Próximos Passos

### Hoje (5 min)
```
✅ Ler este arquivo
✅ Copiar .gitignore para seu projeto
✅ Criar .env.example
```

### Esta Semana (1-2 horas)
```
✅ Instalar pre-commit
✅ Audit seu repositório
✅ Treinar equipe
```

### Este Mês (4-8 horas)
```
✅ Implementar ferramentas
✅ Setup monitoramento
✅ Documentação customizada
```

---

<div align="center">

## 🏆 Você agora sabe:

```
✅ Por que .env nunca é commitado
✅ Por que *.tfvars é crítico
✅ Como proteger chaves privadas
✅ Como recuperar se vazou
✅ Como automatizar segurança
✅ Como documentar corretamente
✅ Como treinar a equipe
✅ Como responder a incidentes
```

### Próximo passo?

👉 [QUICK-START.md](./QUICK-START.md) - Implemente agora!

---

**Tempo para ler:** 15 minutos

**Tempo para implementar:** 1 hora

**Benefício:** Proteção contra $50K+ em incidentes

**Versão:** 1.0.0 | Dezembro de 2025

</div>
