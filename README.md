# 🔐 Git Ignore Security - DevSecOps Best Practices

> Uma documentação abrangente sobre segurança de repositórios Git com foco em proteger dados sensíveis e credenciais

---

## 📌 Sobre Este Projeto

Este repositório fornece **guias práticos e listas completas** de arquivos que devem ser ignorados no Git para evitar comprometimento de segurança. Seguindo as melhores práticas de **DevSecOps** e **Cibersegurança**, você protege suas credenciais, chaves e dados sensíveis de serem expostos publicamente.

---

## 🎯 Por Que Isso Importa?

### O Risco Real

- **Vazamento de Credenciais:** Senhas, tokens e chaves API expostos no Git podem ser explorados por atacantes
- **Acesso Não Autorizado:** Credentials commitadas permitem acesso a seus servidores, bancos de dados e serviços cloud
- **Conformidade:** Violações regulatórias (GDPR, PCI-DSS, HIPAA) por exposição de dados sensíveis
- **Reputação:** Um único vazamento pode danificar a confiança dos clientes e parceiros
- **Custo:** Incidentes de segurança resultam em custos significativos de remediação

### Estatísticas Alarmantes

- 🚨 **Milhões** de secrets são expostos em repositórios públicos anualmente
- ⚠️ A maioria das brechas inclui credenciais encontradas no controle de versão
- 📊 Tempo médio para detectar exposição: **dias a semanas**

---

## 🚀 Como Usar Este Repositório

### 1. **Início Rápido**

```bash
# Clone este repositório
git clone https://github.com/seu-usuario/gitIgnore-security.git

# Copie o .gitignore para seu projeto
cp .gitignore seu-projeto/
```

### 2. **Consulte a Documentação**

- 📖 **[git-list-ignore.md](./git-list-ignore.md)** - Lista completa e categorizada de arquivos para ignorar
- 🛡️ **[README.md](./README.md)** - Este arquivo (guia geral)

### 3. **Configure Seu Projeto**

```bash
# Crie um .gitignore na raiz do seu projeto
cat > .gitignore << 'EOF'
# Credenciais
.env
.env.local
.env.*.local
secrets.txt
credentials.json

# Infrastructure as Code
*.tfvars
*.tfvars.json
.terraform/
terraform.tfstate*

# Chaves
*.pem
*.key
*.crt
id_rsa
id_rsa.pub

# Cloud
.aws/
.gcp/
kubeconfig

# Dependências
node_modules/
venv/
vendor/
EOF
```

---

## 📚 Categorias Principais

### 🔐 Credenciais e Secrets
Arquivo: `.env`, `.env.local`, `secrets.json`, `credentials.yml`

```gitignore
.env
.env.local
.env.*.local
secrets.txt
credentials.json
```

**Exemplo perigoso:**
```
# ❌ NUNCA FAÇA ISSO
DB_PASSWORD=super_secret_123
API_KEY=sk_live_abcd1234efgh5678
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

### 📦 Infrastructure as Code (Terraform)
Arquivo: `*.tfvars`, `terraform.tfstate`

```gitignore
*.tfvars
*.tfvars.json
.terraform/
terraform.tfstate
terraform.tfstate.*
.terraform.lock.hcl
```

**Por quê:** `.tfvars` contém todos os valores das variáveis incluindo credenciais, chaves, senhas de banco de dados.

### 🔑 Chaves e Certificados
Arquivo: `id_rsa`, `*.pem`, `*.key`, `*.crt`

```gitignore
id_rsa
id_rsa.pub
id_dsa
id_dsa.pub
*.pem
*.key
*.crt
*.cer
*.p12
*.pfx
```

**Crítico:** Chaves privadas SSH/GPG nunca devem estar no repositório!

### ☁️ Cloud Providers
Arquivo: `.aws/`, `google-cloud-key.json`, `kubeconfig`

```gitignore
.aws/
.gcp/
.azure/
kubeconfig
google-cloud-key.json
service-account-key.json
```

### 🏗️ Build e Dependências
Arquivo: `node_modules/`, `venv/`, `target/`, `vendor/`

```gitignore
node_modules/
venv/
env/
target/
vendor/
build/
dist/
```

### 📜 Logs e Cache
Arquivo: `*.log`, `logs/`, `cache/`

```gitignore
*.log
logs/
cache/
.tmp/
.cache/
```

---

## 🛠️ Ferramentas Recomendadas

### Git Secret
Encrypt secrets em seu repositório:

```bash
# Instalar (macOS)
brew install git-secret

# Instalar (Linux)
sudo apt-get install git-secret

# Inicializar
git secret init

# Adicionar arquivo ao secret
git secret add .env

# Encryptar
git secret hide

# Decryptar
git secret reveal
```

### Pre-commit Hooks
Previna commits de secrets:

```bash
# Instalar pre-commit
pip install pre-commit

# Criar .pre-commit-config.yaml
cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
EOF

# Instalar hooks
pre-commit install
pre-commit run --all-files
```

### SOPS (Secrets Operations)
Gerenciar secrets com encriptação:

```bash
# Instalar
brew install sops

# Criar arquivo secrets.yaml encriptado
sops --encrypt secrets.yaml
```

### TruffleHog
Escanear repositórios em busca de secrets:

```bash
# Instalar
pip install truffleHog

# Scan
truffleHog filesystem /path/to/repo

# Scan GitHub
truffleHog github --org seu-org
```

---

## 📋 Checklist de Configuração

Use este checklist para garantir que seu projeto está seguro:

### Segurança Básica
- [ ] `.env` está no `.gitignore`
- [ ] `secrets.json` está no `.gitignore`
- [ ] `credentials.yml` está no `.gitignore`
- [ ] `.env.example` existe como template
- [ ] `.env.example` contém apenas valores fake/vazios

### Infrastructure as Code
- [ ] `*.tfvars` está no `.gitignore`
- [ ] `terraform.tfstate*` está no `.gitignore`
- [ ] `.terraform/` está no `.gitignore`
- [ ] `*.auto.tfvars` está no `.gitignore`

### Chaves e Certificados
- [ ] `*.pem` está no `.gitignore`
- [ ] `*.key` está no `.gitignore`
- [ ] `id_rsa` está no `.gitignore`
- [ ] `*.p12`, `*.pfx` estão no `.gitignore`
- [ ] Nenhuma chave privada foi commitada no histórico

### Cloud Providers
- [ ] `.aws/` está no `.gitignore`
- [ ] `.gcp/` está no `.gitignore`
- [ ] `.azure/` está no `.gitignore`
- [ ] `kubeconfig` está no `.gitignore`
- [ ] Arquivos de service accounts estão ignorados

### Banco de Dados
- [ ] `*.db` está no `.gitignore`
- [ ] `dump.sql` está no `.gitignore`
- [ ] `database.yml` com credenciais reais está ignorado

### IDE e Ferramentas
- [ ] `.vscode/` está no `.gitignore`
- [ ] `.idea/` está no `.gitignore`
- [ ] Arquivos temporários de editor estão ignorados

### Verificação Final
- [ ] `.gitignore` está commitado
- [ ] Nenhum secret recente foi commitado
- [ ] GitHub/GitLab scanning está ativado
- [ ] Pre-commit hooks estão instalados (opcional)

---

## 🚨 Resposta a Incidentes

### Secrets Foram Commitados?

Se você descobrir que secrets foram commitados, siga estes passos:

#### 1️⃣ **Avaliação Imediata**

```bash
# Identifique o commit
git log -p | grep -i "password\|token\|key"

# Veja quando foi commitado
git log --oneline | head -20
```

#### 2️⃣ **Revogue Imediatamente**

```bash
# Revogue credenciais no seu serviço
# - AWS: Revogue access keys
# - GitHub: Revogue tokens
# - Databases: Mude senhas
# - APIs: Regenere chaves
```

#### 3️⃣ **Remova do Histórico**

**Opção A - git-filter-repo (Recomendado):**

```bash
# Instalar
pip install git-filter-repo

# Remover arquivo sensível
git filter-repo --invert-paths --path .env

# Force push
git push origin --force-with-lease
```

**Opção B - BFG Repo-Cleaner:**

```bash
# Instalar
brew install bfg

# Remover arquivo
bfg --delete-files .env

# Limpar e push
git reflog expire --expire=now --all
git gc --prune=now --aggressive
git push origin --force-with-lease
```

#### 4️⃣ **Notifique a Equipe**

- Avise todos os desenvolvedores
- Instrua pull com `git pull --rebase origin main`
- Refaça qualquer trabalho em branches afetadas

#### 5️⃣ **Documente**

```bash
# Crie um documento do incidente
# - O que foi exposto
# - Quanto tempo foi exposto
# - Ações tomadas
# - Como prevenir no futuro
```

---

## 📖 Exemplos Práticos

### Exemplo 1: Node.js + Terraform

```gitignore
# Node
node_modules/
npm-debug.log
yarn-error.log

# Env
.env
.env.local
.env.*.local

# Terraform
*.tfvars
terraform.tfstate*
.terraform/

# SSH Keys
*.pem
id_rsa

# IDE
.vscode/
.idea/
```

### Exemplo 2: Python + AWS

```gitignore
# Python
venv/
env/
.venv
*.egg-info/

# Env
.env
.env.local

# AWS
.aws/
aws_credentials
~/.aws/config

# IDE
.vscode/
.idea/
.pyc
```

### Exemplo 3: Kubernetes + Secrets

```gitignore
# Kubernetes
kubeconfig
kubeconfig.yaml
secrets.yaml

# Helm
helm-values-prod.yaml
helm-secrets.yaml

# Env
.env
.env.*.local

# Keys
*.key
*.pem
id_rsa
```

---

## 🔍 Verificação Automática

### GitHub - Dependabot e Secret Scanning

1. **Ativar Secret Scanning:**
   ```
   Settings → Security & analysis → Secret scanning → Enable
   ```

2. **Configurar Branch Protection:**
   ```
   Settings → Branches → Add rule → Require branches to be up to date
   ```

### GitLab - Scanning de Secrets

```yaml
# .gitlab-ci.yml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml
```

### Local - Pre-commit Hooks

```bash
# Instalar detect-secrets
pip install detect-secrets

# Criar arquivo de configuração
cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
EOF

pre-commit install
```

---

## 📚 Recursos Adicionais

### Documentação Oficial
- [Git Documentation - .gitignore](https://git-scm.com/docs/gitignore)
- [GitHub - gitignore Templates](https://github.com/github/gitignore)
- [Terraform - State Management](https://www.terraform.io/docs/language/state/sensitive-data.html)

### Segurança
- [OWASP - Secrets Management](https://owasp.org/www-community/Sensitive_Data_Exposure)
- [SANS - Secrets Management](https://www.sans.org/reading-room/)
- [CWE-798: Use of Hard-Coded Credentials](https://cwe.mitre.org/data/definitions/798.html)

### Ferramentas
- [Git Secret](https://git-secret.io/)
- [SOPS - Secrets Operations](https://github.com/mozilla/sops)
- [TruffleHog - Secret Scanner](https://github.com/trufflesecurity/truffleHog)
- [Detect Secrets](https://github.com/Yelp/detect-secrets)
- [Pre-commit Framework](https://pre-commit.com/)

---

## 🤝 Contribuindo

Quer adicionar mais dicas ou melhorias?

1. Fork este repositório
2. Crie uma branch: `git checkout -b feature/sua-melhoria`
3. Commit suas mudanças: `git commit -am 'Add: nova dica de segurança'`
4. Push para a branch: `git push origin feature/sua-melhoria`
5. Abra um Pull Request

---

## 📜 Licença

Este projeto é licenciado sob a licença MIT - veja o arquivo [LICENSE](./LICENSE) para detalhes.

---

## 🔗 Links Rápidos

| Link | Descrição |
|------|-----------|
| [git-list-ignore.md](./git-list-ignore.md) | 📋 Lista completa de arquivos para ignorar |
| [GitHub GitIgnore Templates](https://github.com/github/gitignore) | 📦 Templates oficiais do GitHub |
| [OWASP Top 10](https://owasp.org/www-project-top-ten/) | 🛡️ Top 10 vulnerabilidades web |
| [Terraform Best Practices](https://www.terraform.io/docs/cloud/) | ☁️ Boas práticas Terraform |

---

## 💡 Dicas de Ouro

### ✅ Faça Isso

```bash
# ✅ Crie templates de configuração
.env.example
.aws.config.example
terraform.example.tfvars

# ✅ Use gerenciadores de secrets
AWS Secrets Manager
HashiCorp Vault
GitHub Secrets

# ✅ Ative verificação automática
GitHub Secret Scanning
GitLab Secret Detection
Pre-commit hooks
```

### ❌ Nunca Faça Isso

```bash
# ❌ Commite secrets
password = "super_secret_123"
API_KEY = "sk_live_1234567890"

# ❌ Ignore o .gitignore
git add -f .env

# ❌ Deixe credenciais em comentários
# Password: admin123

# ❌ Envie para repositórios públicos
git push origin main  # se tem secrets
```

---

## 📞 Suporte

Encontrou um problema ou tem dúvidas?

- 📧 Abra uma issue neste repositório
- 💬 Participe das discussões
- 🔗 Envie um pull request com melhorias

---

## 🎓 Educação Contínua

Mantenha sua equipe informada sobre segurança:

1. **Workshops Mensais:** Discuta novos riscos e ferramentas
2. **Code Reviews:** Sempre revise `.gitignore` e configurações
3. **Testes:** Execute scanners regularmente
4. **Documentação:** Mantenha runbooks de resposta a incidentes

---

<div align="center">

### 🔐 Segurança é Responsabilidade de Todos

**Dedique tempo para configurar corretamente desde o início do projeto.**

```
💻 DevSecOps = Development + Security + Operations
```

Última atualização: **Novembro de 2025**

![Security Badge](https://img.shields.io/badge/Security-First-brightgreen?style=flat-square)
![Git Badge](https://img.shields.io/badge/Git-Best%20Practices-blue?style=flat-square)
![DevSecOps Badge](https://img.shields.io/badge/DevSecOps-Certified-orange?style=flat-square)

</div>
