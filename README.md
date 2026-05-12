# 🔐 CI/CD com SAST — SonarCloud + Trivy + GitHub Actions

Projeto de demonstração para a live do curso **Devsecops IMPACTA**.

---

## 🎯 Objetivo

Demonstrar como incorporar **segurança e qualidade de código** desde o início do desenvolvimento (**Shift Left Security**) usando SonarCloud integrado ao GitHub Actions, com análise automática a cada Pull Request.

---

## 🧠 Conceitos

| Tipo | Sigla | O que analisa |
|------|-------|---------------|
| Static Application Security Testing | **SAST** | Código-fonte |
| Dynamic Application Security Testing | **DAST** | App em execução |
| Software Composition Analysis | **SCA** | Dependências/libs |
| Infrastructure as Code Security | **IaC Sec** | Dockerfile, YAML |
| Container Security | **CS** | Imagens Docker |

---

## 🛠️ Stack

- **SonarCloud** — SAST + Code Quality + Quality Gate
- **Trivy** — Container Scan + IaC Scan
- **GitHub Actions** — Orquestração da pipeline

---

## 📁 Estrutura

```
.
├── .github/
│   └── workflows/
│       └── sast-scan.yml          # Pipeline de segurança
├── app/
│   ├── app.py                     # App Python (vulnerabilidades intencionais)
│   ├── test_app.py                # Testes unitários
│   ├── requirements.txt           # Dependências (versões antigas para demo)
│   └── Dockerfile                 # Imagem Docker
├── sonar-project.properties       # Configuração do SonarCloud
├── .gitignore
└── README.md
```

---

## ⚙️ Configuração

### 1. SonarCloud

1. Acesse [sonarcloud.io](https://sonarcloud.io) e faça login com GitHub
2. Adicione o repositório → **"With GitHub Actions"**
3. Copie o `SONAR_TOKEN` gerado
4. No GitHub: **Settings → Secrets → Actions → New repository secret**
   - Name: `SONAR_TOKEN`
   - Value: token copiado
5. Edite `sonar-project.properties` com seu **Project Key** e **Organization Key**

### 2. Branch Protection (para bloquear merge)

**Settings → Branches → Add rule → main**
- ✅ Require status checks to pass before merging
- Adicione: `SonarCloud Code Analysis`

---

## 🚀 Pipeline

```
Security Scan — SonarCloud + Trivy
├── SonarCloud SAST          (paralelo)
├── Trivy IaC + Dockerfile   (paralelo)
└── Trivy Container Scan     (após os dois anteriores)
```

---

## 🔴 Vulnerabilidades Intencionais (app.py)

| # | Tipo | Linha | OWASP |
|---|------|-------|-------|
| 1 | SQL Injection | 12 | A03:2021 |
| 2 | Command Injection | 20 | A03:2021 |
| 3 | Hardcoded Secrets | 24-25 | A07:2021 |
| 4 | debug=True em produção | 36 | A05:2021 |

> ⚠️ As dependências em `requirements.txt` também são intencionalmente antigas para demonstrar CVEs com o Trivy.

---

## ✅ Fluxo da Live

1. **Push inicial** → pipeline executa → vulnerabilidades detectadas
2. **Criar PR** com nova vulnerabilidade (Path Traversal) → Quality Gate falha → merge bloqueado
3. **Corrigir** todas as vulnerabilidades → Quality Gate passa → merge liberado

---

**Professor:** Thiago Alves Gomes
**Curso:** DevSecOps — IMPACTA  
**Tema:** CI/CD com SAST — SonarCloud + Trivy + GitHub Actions

---

**🔐 Secure by Default. Shift Left. Ship Fast.**
