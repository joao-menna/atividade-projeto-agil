# Runbook — Rollback Seguro

1. Identificar versão estável anterior (`git tag` ou release anterior).
2. Executar rollback via pipeline: `deploy --version <tag-anterior>`.
3. Validar:
   - Testes de fumaça (login, checkout, API /health)
   - Logs e métricas normalizados
4. Comunicar status no canal #release-status.
5. Criar issue de post-mortem com causa e ação corretiva.

*Tempo esperado:* < 15 minutos.
