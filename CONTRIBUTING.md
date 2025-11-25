# 🤝 Como Contribuir - Git Ignore Security

> Diretrizes para contribuir com melhorias neste projeto

---

## 📋 Antes de Começar

- Leia todo este documento
- Estude a [Estrutura do Projeto](./OVERVIEW.md)
- Revise issues existentes
- Consulte [LICENSE](./LICENSE) (MIT)

---

## 🎯 Tipos de Contribuição

### 📖 Documentação
- Melhorar exemplos
- Adicionar novos padrões
- Corrigir erros
- Traduzir para outros idiomas

### 🔧 Arquivos
- Melhorar `.gitignore`
- Atualizar `.env.example`
- Criar novos templates

### 🐛 Bugs
- Relatar informações incorretas
- Encontrar links quebrados
- Identificar padrões incompletos

### 💡 Features
- Novas seções
- Novas ferramentas
- Novos checklists

### 🎓 Educação
- Criar tutoriais
- Adicionar exemplos
- Melhorar explanações

---

## 🚀 Começando

### 1. Fork e Clone

```bash
# Fork em GitHub
# (Botão "Fork" no topo)

# Clone seu fork
git clone https://github.com/seu-usuario/gitIgnore-security.git
cd gitIgnore-security

# Adicione upstream
git remote add upstream https://github.com/original/gitIgnore-security.git
```

### 2. Crie uma Branch

```bash
# Atualize main
git fetch upstream
git checkout main
git merge upstream/main

# Crie sua branch
git checkout -b feature/descricao-clara
# ou
git checkout -b fix/bug-descricao
# ou
git checkout -b docs/melhorias-descricao
```

### 3. Faça suas Mudanças

Siga os [Padrões de Código](#-padrões-de-código)

### 4. Commit e Push

```bash
# Verifique mudanças
git status

# Commit com mensagem clara
git commit -m "docs: add new pattern for X

- Descrição clara do que foi adicionado
- Por que é importante
- Link para referência (se aplicável)"

# Push
git push origin sua-branch
```

### 5. Abra Pull Request

- Descreva claramente o que foi feito
- Referencie issues se aplicável
- Verifique checklist abaixo

---

## 📝 Padrões de Código

### Commits
```bash
# Formato
<tipo>: <descrição curta>

<descrição detalhada>

# Tipos
docs:    Documentação
feat:    Nova feature
fix:     Correção
test:    Testes
refactor: Refatoração
style:   Formatação
chore:   Build, deps

# Exemplo
docs: add kubernetes secret examples

- Added examples for kubeconfig
- Added examples for helm-values
- Linked to relevant security docs
```

### Branches
```
Feature:      feature/<descrição>
Fix:          fix/<descrição>
Documentação: docs/<descrição>

Exemplos:
- feature/add-gitleaks-tool
- fix/incorrect-terraform-pattern
- docs/improve-aws-section
```

### Nomes de Arquivo
```
- Minúsculas (exceto siglas: AWS, GCP, K8s)
- Hífens para separador (não underscore)
- Descritivos e claros
- Sem acentos
```

### Markdown
```markdown
# Formato

## Seção Principal
### Subsection
#### Sub-subsection

**bold** para destaque
`code` para código inline
[link](url) para links

- [ ] Checklist items
```

---

## ✅ Checklist do Pull Request

Antes de submeter PR:

### Conteúdo
- [ ] Conteúdo é preciso e útil
- [ ] Segue padrões do projeto
- [ ] Tem exemplos práticos
- [ ] Links estão funcionando
- [ ] Sem informações sensíveis
- [ ] Referenciado em INDEX.md se necessário

### Qualidade
- [ ] Sem erros de digitação
- [ ] Sem erros gramaticais
- [ ] Formatação consistente
- [ ] Indentação correta
- [ ] Sem trailing whitespace

### Documentação
- [ ] Atualizado README.md se necessário
- [ ] Atualizado INDEX.md se necessário
- [ ] Descrição clara do PR
- [ ] Referenciar issues

### Segurança
- [ ] Sem secrets nos exemplos
- [ ] Sem credenciais reais
- [ ] Usando valores fake/example
- [ ] Recomendações foram revisadas

---

## 🐛 Reportando Bugs

### Título Claro
```
❌ "Algo não funciona"
✅ "Padrão .env.example não inclui NODE_ENV"
```

### Descrição Completa
```markdown
## Descrição
Descreva o bug

## Localização
Arquivo e seção afetada

## Esperado
O que deveria acontecer

## Atual
O que está acontecendo

## Exemplo
// Código ou exemplo que demonstra o bug

## Sugestão de Correção
Sua ideia de solução
```

### Exemplo
```markdown
## Descrição
Padrão de Kubernetes no .gitignore não está correto

## Localização
git-list-ignore.md - Seção Kubernetes

## Esperado
kubeconfig deve estar ignorado

## Atual
kubeconfig.yaml está, mas kubeconfig não

## Sugestão
Adicionar:
```
kubeconfig
kubeconfig.yaml
kubeconfig.yml
```
```

---

## 💡 Solicitações de Features

### Título Claro
```
❌ "Adicionar mais coisa"
✅ "Adicionar ferramentas para scanning de SBOM"
```

### Descrição
```markdown
## Descrição
Por que essa feature é importante

## Caso de Uso
Quando seria usada

## Exemplo
Como funcionaria

## Referência
Links relevantes
```

### Exemplo
```markdown
## Descrição
Adicionar seção sobre Sigstore e SBOM scanning

## Caso de Uso
Verificação de integridade de artifacts e dependências

## Exemplo
- Syft para geração de SBOM
- Cosign para assinatura
- Grype para scanning

## Referência
https://github.com/sigstore
```

---

## 📚 Áreas para Contribuição

### Muito Procurado
- [ ] Exemplos em outras linguagens
- [ ] Integração com ferramentas novas
- [ ] Traduções para outros idiomas
- [ ] Casos de uso reais
- [ ] Automação em CI/CD
- [ ] Comparação de ferramentas
- [ ] Templates adicionais

### Procurado
- [ ] Melhorias no README
- [ ] Novos patterns .gitignore
- [ ] Novos itens de checklist
- [ ] Exemplos de erro comum
- [ ] Links para recursos
- [ ] Corrigir formatação
- [ ] Clarificar explicações

### Nice-to-Have
- [ ] Pequenas melhorias
- [ ] Correção ortográfica
- [ ] Reordenamento
- [ ] Novas badges
- [ ] Visuais melhorados

---

## 🎓 Guia de Estilo

### Tom e Voz
- Profissional mas acessível
- Claro e conciso
- Educativo, não condescendente
- Prático e aplicável

### Exemplos
```markdown
❌ "Você deve ignorar isso porque a segurança é importante"
✅ "Ignore .env para evitar expor credenciais de banco de dados"

❌ "Use esta ferramenta complexa"
✅ "Use X para detectar secrets (instalação: brew install x)"
```

### Estrutura de Seção
```markdown
## 🔐 Título da Seção

Introdução/contexto em 1-2 linhas.

### Sub-tópico

Explicação detalhada.

**Exemplo:**
```código```

**Quando usar:** Contexto de uso
**Alternativas:** Outras opções
**Recursos:** Links úteis
```

---

## 🔄 Processo de Review

1. **Submissão**: Você abre PR
2. **Verificação**: Automática de CI/CD
3. **Review**: Pelo menos 1 mantainer
4. **Feedback**: Comentários se necessário
5. **Ajustes**: Você faz mudanças se needed
6. **Aprovação**: Review aprovado
7. **Merge**: Seu PR é mergeado!

### Feedback Comum

```
✅ Aceito
→ "Looks great! Merging..."

💭 Precisa revisão
→ "Could you clarify...?"

❌ Mudanças solicitadas
→ "Please address these points"
```

---

## 🛠️ Desenvolvendo Localmente

### Setup
```bash
git clone seu-fork
cd gitIgnore-security

# Abra em seu editor
code .
# ou
vim README.md
```

### Testando
```bash
# Links
- Verifique links em markdown
- Use `npm install -g markdown-link-check`
- `markdown-link-check *.md`

# Formatação
- Use markdown linter
- `npm install -g markdownlint-cli`
- `markdownlint *.md`

# Ortografia
- Use spell checker
- `npm install -g cspell`
- `cspell *.md`
```

### Visualizando Markdown
```bash
# VS Code
- Install Markdown Preview Enhanced

# Online
- GitHub preview (mais simples)

# Localmente
- `npm install -g http-server`
- `http-server`
```

---

## 📋 Checklist Final

Antes de submeter PR:

```bash
# Verify
✓ Branch is based on latest main
✓ Commits are clean
✓ No merge conflicts
✓ CI/CD passed
✓ No secrets in examples
✓ Links are working
✓ Markdown is valid
✓ Spelling is correct
✓ Style is consistent
✓ References are added
✓ Checklist completed
```

---

## 🎉 Após Merge

Parabéns! 🎊

- Seu commit está no repositório
- Você está listado em contribuidores
- Sua mudança ajuda a comunidade
- Obrigado! 🙏

---

## 📞 Perguntas?

### Stack Overflow
Tag: `git-security` ou `devsecops`

### Discussions
Abra uma discussion para perguntas

### Issues
Use issues para bugs/features

### Email
Veja SECURITY.md para responsabilidade disclosure

---

## 📚 Leitura Adicional

- [GitHub Contributing Guide](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Keep a Changelog](https://keepachangelog.com/)

---

## 🏆 Contribuidores

Este projeto é possível graças aos contribuidores:

- Veja [CONTRIBUTORS.md](./CONTRIBUTORS.md)

Você pode ser o próximo! 👋

---

## ⭐ Apoie o Projeto

Se achou útil:

- ⭐ Star no GitHub
- 🔄 Share com a comunidade
- 💬 Deixe feedback
- 🤝 Contribua com melhorias

---

<div align="center">

### Obrigado por Contribuir! 🙏

Juntos tornamos a segurança melhor para todos.

**Última atualização:** Novembro de 2025

</div>
