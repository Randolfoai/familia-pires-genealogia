# Passo a passo para amanhã (outro computador)

## 1. Levar a pasta de fotos
Leve o zip do diretório local de trabalho (que já inclui `Fotos do Livro` e `Novas Fotos`) e extraia no novo computador mantendo a mesma estrutura de pastas, idealmente no mesmo caminho:

```
D:\IABRACADABRA\0058 - Família Pires\...
```

Se o caminho for diferente lá, é só me avisar assim que começarmos.

## 2. Obter os arquivos do projeto via git
Se o repositório ainda não existir no novo computador:

```bash
git clone https://github.com/Randolfoai/familia-pires-genealogia.git
```

Se já existir uma cópia do repositório lá (de uma sessão anterior):

```bash
git pull origin main
```

## 3. Abrir o Claude Code na pasta do projeto
Abra o Claude Code apontando para a pasta clonada/atualizada (a que contém o `CLAUDE.md`). Isso já garante que eu leia automaticamente o `IDENTIDADE_VISUAL.md` antes de gerar qualquer infográfico novo.

## 4. Me dar o contexto rápido
Como minha memória desta sessão não viaja para outro computador, é útil você me dizer algo como: "Continuando o trabalho dos infográficos de genealogia da família Pires — leia o IDENTIDADE_VISUAL.md". Eu recupero o resto (padrões, regras, histórico) a partir dos arquivos do próprio repositório.

## 5. Conferir se está tudo certo
Pode pedir pra eu rodar `git status` e `git log --oneline -5` pra confirmar que peguei a versão mais recente antes de começarmos a gerar algo novo.

## 6. Sobre push/commit
Se quiser manter o mesmo fluxo (commit a cada infográfico, push só ao encerrar o dia), é só me lembrar dessa preferência — como cada sessão não herda memória automaticamente, vale repetir esse combinado no início.
