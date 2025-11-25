# 🔐 Checklist de Segurança para Repositórios Git

> Uma checklist pronta para auditar seus repositórios e garantir que nenhum secret foi exposto

---

## 📋 Checklist Pré-Commit (Antes de Fazer Push)

### Credenciais Básicas
- [ ] Nenhuma senha foi commitada em código
- [ ] Nenhuma chave API foi commitada
- [ ] Nenhum token de autenticação foi commitada
- [ ] Arquivos `.env` estão no `.gitignore`
- [ ] Arquivo `.env.example` existe como template

### Infrastructure as Code
- [ ] `*.tfvars` está no `.gitignore`
- [ ] `terraform.tfstate*` está no `.gitignore`
- [ ] `.terraform/` está no `.gitignore`
- [ ] Nenhum arquivo de variáveis sensíveis foi commitado
- [ ] Estado do Terraform está armazenado remotamente (S3, TFCloud)

### Chaves e Certificados
- [ ] SSH keys (`id_rsa`, `id_ed25519`) não foram commitadas
- [ ] Certificados privados não foram commitados
- [ ] `.pem`, `.key`, `.crt` estão no `.gitignore`
- [ ] Chaves GPG privadas não estão no repositório
- [ ] `kubeconfig` não foi commitado

### Cloud Providers
- [ ] `.aws/` está no `.gitignore`
- [ ] `.gcp/` está no `.gitignore`
- [ ] `.azure/` está no `.gitignore`
- [ ] JSON de credenciais cloud não foram commitados
- [ ] Service accounts keys não foram commitadas

### Banco de Dados
- [ ] Credenciais de DB não estão em código
- [ ] String de conexão com senha não foi commitada
- [ ] Backup/dump do DB não foi commitado
- [ ] Database seeds sensíveis não foram commitados

### Código e Configuração
- [ ] Nenhum hardcoded password/token no código
- [ ] Nenhuma credencial em comentários
- [ ] Configurações de prod não estão no repositório
- [ ] Logs com informações sensíveis não foram commitados

---

## 🔍 Checklist Pós-Commit (Verificação Histórica)

### Análise do Histórico
- [ ] Executou `git log -p | grep -i "password\|api_key\|secret"`
- [ ] Verificou o histórico de arquivos deletados
- [ ] Consultou branches não mergeadas
- [ ] Checou tags e releases

### Detecção de Secrets
- [ ] Rodou `truffleHog` no repositório
- [ ] Executou `git-secrets` em todos os commits
- [ ] Usou `detect-secrets` para scanning
- [ ] Revisor também verificou com ferramentas

### Verificação de Acesso
- [ ] Repositório tem branch protection ativada
- [ ] Apenas membros autorizados podem fazer push
- [ ] Histórico de quem fez push foi verificado
- [ ] Logs de acesso foram consultados

---

## 🛠️ Checklist de Configuração (Setup Inicial)

### Git Ignore
- [ ] `.gitignore` existe na raiz do projeto
- [ ] `.gitignore` foi commitado
- [ ] `.gitignore` inclui `.env*` 
- [ ] `.gitignore` inclui `*.tfvars`
- [ ] `.gitignore` inclui chaves privadas
- [ ] `.gitignore` inclui configurações sensíveis
- [ ] `.gitignore` foi revisado pela equipe

### Git Secrets
- [ ] `git-secrets` foi instalado
- [ ] Hooks foram instalados: `git secrets --install`
- [ ] Regras customizadas foram adicionadas
- [ ] Todos na equipe têm instalado
- [ ] Testes rodaram com sucesso

### Pre-commit Hooks
- [ ] `pre-commit` framework está instalado
- [ ] `.pre-commit-config.yaml` existe
- [ ] Detect-secrets está configurado
- [ ] Hooks foram instalados: `pre-commit install`
- [ ] Executou test run: `pre-commit run --all-files`

### GitHub/GitLab Security
- [ ] Secret Scanning está ativado
- [ ] Branch protection está ativada
- [ ] Require status checks está ativado
- [ ] Require reviews está ativado
- [ ] Admin review está ativado
- [ ] Dismiss stale reviews está ativado

### CI/CD Security
- [ ] Segredos estão em CI/CD Secrets, não no código
- [ ] Logs de CI/CD não expõem secrets
- [ ] Artifacts não contêm informações sensíveis
- [ ] Acesso a CI/CD é restrito
- [ ] Audit logs de CI/CD estão ativados

---

## 🚨 Checklist de Incidente (Se Secret Foi Exposto)

### Resposta Imediata
- [ ] Revogou credenciais comprometidas
- [ ] Mudou senhas de contas afetadas
- [ ] Regenerou API keys
- [ ] Revogou tokens/OAuth
- [ ] Notificou a equipe de segurança

### Remediação
- [ ] Removeu arquivo do histórico Git (git-filter-repo)
- [ ] Force push foi feito com `--force-with-lease`
- [ ] Comunicou com toda a equipe
- [ ] Solicitou pull/rebase para todos
- [ ] Verificou clones locais dos desenvolvedores

### Investigação
- [ ] Documentou o que foi exposto
- [ ] Determinou por quanto tempo ficou exposto
- [ ] Verificou logs de acesso ao repositório
- [ ] Checou se foi clonado por outros
- [ ] Analisou se foi utilizado

### Prevenção Futura
- [ ] Criou issue para melhorar checklist
- [ ] Revisou `.gitignore`
- [ ] Implementou ferramentas de scanning
- [ ] Treinou equipe
- [ ] Documentou lessons learned
- [ ] Atualizou políticas de segurança

---

## 📊 Checklist por Tipo de Projeto

### Node.js + Express + MongoDB

- [ ] `.env` no `.gitignore`
- [ ] `node_modules/` no `.gitignore`
- [ ] Credenciais de MongoDB não estão em código
- [ ] JWT secrets estão em `.env`
- [ ] API keys de terceiros estão em `.env`
- [ ] `.npmrc` com tokens não foi commitado
- [ ] `package-lock.json` foi revisado
- [ ] Build artifacts não foram commitados

### Python + Django

- [ ] `venv/` está no `.gitignore`
- [ ] `settings/local.py` está no `.gitignore`
- [ ] Database password em `.env`
- [ ] SECRET_KEY em `.env` (gerado com `get_random_secret_key()`)
- [ ] `__pycache__/` está no `.gitignore`
- [ ] `.pyc` arquivos estão no `.gitignore`
- [ ] Credentials de AWS/GCP estão em `.env`

### Terraform + AWS

- [ ] `*.tfvars` está no `.gitignore`
- [ ] `terraform.tfstate*` está no `.gitignore`
- [ ] `.terraform/` está no `.gitignore`
- [ ] Remote state está configurado (S3 + DynamoDB)
- [ ] Nenhum hardcoded na variável
- [ ] Variáveis sensíveis usam `sensitive = true`
- [ ] `.terraform.lock.hcl` foi commitado (recomendado)

### Docker + Kubernetes

- [ ] Dockerfile não contém secrets
- [ ] Docker image não foi pusheada com secrets
- [ ] ConfigMaps não contêm dados sensíveis
- [ ] Secrets do K8s estão em Secret Manager, não em YAML
- [ ] `.dockerignore` inclui sensíveis
- [ ] Registry credentials são privadas
- [ ] Image scanning está ativado

### Go + Gin + PostgreSQL

- [ ] `.env` está no `.gitignore`
- [ ] Credenciais de DB estão em `.env`
- [ ] API keys estão em `.env`
- [ ] `vendor/` está no `.gitignore` (se não commitado)
- [ ] Binários compilados não foram commitados
- [ ] Testes não usam dados sensíveis reais
- [ ] Logs de debug não contêm secrets

---

## 🔐 Checklist de Hardening (Melhorando Segurança)

### Implementação de Ferramentas

- [ ] GitHub Secret Scanning ativado
  ```
  Settings → Security & analysis → Secret scanning
  ```

- [ ] Dependabot habilitado
  ```
  Settings → Security & analysis → Dependabot
  ```

- [ ] Branch protection configurada
  ```
  Settings → Branches → Add rule
  ```

- [ ] CODEOWNERS arquivo criado
  ```
  Create CODEOWNERS file
  ```

- [ ] Pre-commit hooks instalados
  ```bash
  pip install pre-commit
  pre-commit install
  ```

- [ ] Git-secrets instalado
  ```bash
  brew install git-secrets
  git secrets --install
  git secrets --register-aws
  ```

### Políticas e Procedimentos

- [ ] Documentação de secrets management criada
- [ ] Onboarding de novos devs inclui security checklist
- [ ] Code review checklist inclui segurança
- [ ] PR template inclui lembrete de secrets
- [ ] Equipe teve training em segurança
- [ ] Runbook de incidente foi criado

### Monitoramento

- [ ] Audit logs estão ativados
- [ ] Repository insights estão configurados
- [ ] Alerts de security estão habilitadas
- [ ] Integração com Slack foi feita
- [ ] Dashboard de segurança criado
- [ ] Relatório mensal de segurança agendado

---

## 📱 Checklist Móvel (Mobile Apps)

### iOS (Xcode)
- [ ] Keychain usada para armazenar credentials
- [ ] Nenhuma credencial em plist ou código
- [ ] Certificates privados não foram commitados
- [ ] Provisioning profiles não foram commitados
- [ ] API keys estão em Config file ignorado

### Android
- [ ] Local.properties não foi commitado
- [ ] Arquivo keystore não foi commitado
- [ ] Credentials não em strings.xml
- [ ] BuildConfigs com secrets não foram commitados
- [ ] Proguard rules mantêm secrets ofuscados

---

## 🌐 Checklist Cloud-Specific

### AWS
- [ ] AWS Credentials não estão em `~/.aws/credentials` commitado
- [ ] IAM roles usadas em vez de keys
- [ ] KMS usado para dados sensíveis
- [ ] Secrets Manager para application secrets
- [ ] VPC endpoints para APIs privadas
- [ ] CloudTrail ativado para auditoria

### Google Cloud
- [ ] Service account keys não foram commitadas
- [ ] Secret Manager usado para secrets
- [ ] Cloud KMS para criptografia
- [ ] Cloud Audit Logs ativado
- [ ] Workload Identity usada

### Azure
- [ ] Managed Identity usado quando possível
- [ ] Key Vault para secrets
- [ ] Encryption at rest habilitada
- [ ] Azure Monitor logging ativado
- [ ] Role-Based Access Control configurado

---

## ✅ Checklist Final (Antes de Deploy)

- [ ] Toda documentação foi atualizada
- [ ] README inclui security best practices
- [ ] CONTRIBUTING.md inclui security guidelines
- [ ] LICENSE está atualizado
- [ ] SECURITY.md existe com policy de disclosure
- [ ] Dependency check foi executado
- [ ] Code scanning foi executado
- [ ] Security audit final foi feito

---

## 📊 Template de Auditoria

Use este template para documentar sua auditoria:

```markdown
# Auditoria de Segurança - [Projeto] - [Data]

## Resultado Geral
- [x] Passado / [ ] Com ressalvas / [ ] Falhou

## Itens Verificados
- [ ] Secrets não foram encontrados
- [ ] Histórico foi verificado
- [ ] Ferramentas estão configuradas
- [ ] Políticas estão documentadas
- [ ] Equipe está treinada

## Itens de Ação Encontrados
1. ...
2. ...
3. ...

## Próxima Auditoria
Data: [30 dias de agora]
Responsável: [Nome]
```

---

## 🎓 Recursos de Aprendizado

- [GitHub Security Best Practices](https://docs.github.com/en/code-security)
- [OWASP Secrets Management](https://owasp.org/www-community/Sensitive_Data_Exposure)
- [CWE-798: Hard-Coded Credentials](https://cwe.mitre.org/data/definitions/798.html)
- [Terraform Security](https://www.terraform.io/docs/cloud/security/)

---

**Última atualização:** Novembro de 2025

> 🔒 Segurança é um processo contínuo, não um evento único.
