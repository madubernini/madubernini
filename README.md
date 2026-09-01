#!/usr/bin/env bash
# Cria o repositório de perfil do GitHub e sobe o README.
#
# Uso:
#   ./setup-perfil.sh SEU-USUARIO
#
# Requisitos: git. O GitHub CLI (gh) é opcional — se estiver instalado
# e autenticado, o script cria o repositório sozinho.

set -euo pipefail

USUARIO="${1:-}"

if [ -z "$USUARIO" ]; then
  echo "Erro: informe seu usuário do GitHub."
  echo "Uso: $0 SEU-USUARIO"
  exit 1
fi

echo "==> Preparando repositório de perfil para: $USUARIO"

mkdir -p "$USUARIO"
cd "$USUARIO"

# --- README ---------------------------------------------------------------

cat > README.md << EOF
## Maria Eduarda Bernini

Engenheira de Dados. Passo boa parte do tempo transformando dado bruto em algo confiável — e o resto descobrindo por que o pipeline quebrou de madrugada.

Estudante de Análise e Desenvolvimento de Sistemas.

### Stack do dia a dia

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=flat&logo=apachespark&logoColor=white)
![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=flat&logo=databricks&logoColor=white)
![Azure Data Factory](https://img.shields.io/badge/Azure_Data_Factory-0078D4?style=flat&logo=microsoftazure&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=postgresql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)

### O que eu gosto de construir

Automação de processo que hoje é feito na mão. Observabilidade de pipeline — alerta que avisa antes de alguém reclamar. E refatoração daquele código que "funciona, mas ninguém entende".
EOF

echo "==> README.md gerado."

# --- Git ------------------------------------------------------------------

if [ ! -d .git ]; then
  git init -q
  git branch -M main
fi

git add README.md
git commit -q -m "Adiciona README do perfil" || echo "==> Nada novo para commitar."

# --- Repositório remoto ---------------------------------------------------

if command -v gh > /dev/null 2>&1; then
  echo "==> GitHub CLI detectado. Criando repositório e enviando..."
  gh repo create "$USUARIO" --public --source=. --remote=origin --push
else
  echo
  echo "==> GitHub CLI não encontrado. Faça o resto manualmente:"
  echo
  echo "   1. Crie um repositório PÚBLICO chamado exatamente: $USUARIO"
  echo "      https://github.com/new"
  echo "   2. Depois rode, dentro da pasta ./$USUARIO :"
  echo
  echo "      git remote add origin git@github.com:$USUARIO/$USUARIO.git"
  echo "      git push -u origin main"
  echo
fi

echo "==> Pronto. Confira em: https://github.com/$USUARIO"

<div align="center">

<a href="https://www.linkedin.com/in/madubernini" target="_blank">
<img src="https://img.shields.io/badge/LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white">
</a>

</div>
