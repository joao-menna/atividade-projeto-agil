# atividade-projeto-agil
Objetivo: desenhar como o time vai garantir qualidade e entrega com previsibilidade conectando PDCA/Kaizen e maturidade (evidencias leves).

# Convenção de commits

\<tipo\>(\<escopo\>): \<mensagem curta\>

tipos: feat, fix, chore, refactor, test, docs, ci
ex: feat(auth): adiciona refresh token JWT

# Qualidade e testes

## Pirâmide de testes

> E2E (5%) — fluxo completo crítico, via Cypress ou Playwright
> Integração (25%) — API e módulos principais
> Unitários (70%) — funções puras, services, utils

## Cobertura mínima

Meta: >= 80% global, mas com foco em cobertura útil (código relevante).

## Ferramentas

Lint: ESLint + Prettier -> blocking na pipeline
SAST: SonarQube ou CodeQL -> bloqueia se vulnerabilidade alta/crítica.

# CI/CD & Ambientes (Fluxo visível)

## Pipeline mínimo (ci.yml)

```yml
name: CI Pipeline

on: [push, pull_request]

jobs:
  build-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install
        run: npm ci
      - name: Lint
        run: npm run lint
      - name: Test
        run: npm test -- --coverage
      - name: Coverage check
        run: npx check-coverage --statements 80
      - name: SAST
        run: npx sonar-scanner # TODO: configurar token
      - name: Build artifact
        run: npm run build
```

# Releases Seguras & Rollback (Risco controlado)

## Estratégia

- Canary release com feature flags para ativar gradualmente novas funções.
- Deploy progressivo: 5% -> 20% -> 100% tráfego.

## Gatilhos de rollback

- 2% de erros 5XX/minuto
- Queda de >10% no sucesso de E2E
- Latência p95 > 2x baseline

# Observabilidade, DORA & PDCA (Repetibilidade)

## Logs essenciais

1. error.log -> erros de aplicação e stack traces.
1. access.log -> tempo de resposta, status HTTP, endpoint.

## Métricas

- 5XX/min -> taxa de falhas.

- latência p95 -> performance percebida.

## DORA mínima

- Lead time for changes: tempo entre commit no main e deploy em produção (coletado via CI/CD).
- Change failure rate: % de deploys revertidos (dados do rollback + incidentes).

# Rotina PDCA (Kaizen leve)

|Etapa    |Frequência|Responsável   |Ação                                                           |
|---------|----------|--------------|---------------------------------------------------------------|
|Planejar |quinzenal |Tech Lead + PO|Revisar DORA e definir foco                                    |
|Executar |contínuo  |Time de dev   |Aplicar melhorias                                              |
|Verificar|semanal   |DevOps        |Monitorar métricas e logs                                      |
|Agir     |mensal    |Time completo |Retro + registro de aprendizados no repositório `kaizen-log.md`|

