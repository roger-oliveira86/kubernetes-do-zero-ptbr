---
title: "Observabilidade não é dashboard: é suporte à decisão"
status: rascunho
tema: "Como métricas, logs e traces ajudam a avaliar mudança, impacto e próximo passo seguro"
---

# Observabilidade não é dashboard: é suporte à decisão

> Rascunho sanitizado para revisão técnica e editorial. Não contém dados, sistemas ou incidentes identificáveis.

## Tese

Um dashboard pode estar verde e, ainda assim, uma mudança não ser segura.

Monitoramento responde se um limiar foi ultrapassado. Observabilidade precisa ajudar a responder uma pergunta mais difícil: **diante deste comportamento, qual é o próximo passo com menor risco?**

Essa diferença muda o papel da telemetria. Ela deixa de ser apenas uma coleção de gráficos e passa a sustentar decisões de engenharia.

## Contexto

Mudanças em produção são inevitáveis: uma versão nova, uma alteração de configuração, uma dependência que passou a responder de modo diferente ou uma variação de carga.

O risco não está apenas em detectar que algo deu errado. Está em não conseguir distinguir rapidamente entre:

- um desvio sem impacto relevante;
- um problema localizado em uma jornada;
- uma degradação que exige interromper a mudança;
- e um incidente que pede rollback imediato.

Sem esse contexto, o time tende a decidir por percepção, pressão de tempo ou pelo gráfico mais chamativo.

## Decisão ou trade-off

Uma investigação operacional madura começa com quatro perguntas:

1. **O que mudou?**  
   Versão, configuração, dependência, capacidade ou tráfego precisam ser visíveis no mesmo contexto da telemetria.

2. **Quem foi afetado?**  
   Métricas de infraestrutura são necessárias, mas não substituem sinais da jornada do usuário: sucesso, latência, disponibilidade e comportamento de negócio.

3. **Qual é o escopo?**  
   Logs mostram eventos específicos; traces conectam chamadas distribuídas; métricas mostram tendência e proporção. Nenhum desses sinais é suficiente isoladamente.

4. **Qual é a próxima ação segura?**  
   Avançar, pausar, reduzir o escopo, aplicar mitigação ou executar rollback são decisões diferentes. A evidência precisa tornar esses caminhos comparáveis.

O trade-off não é “mais dashboards versus menos dashboards”. É investir em sinais que reduzem incerteza antes que uma mudança aumente o *blast radius*.

## Evidência

Em uma arquitetura distribuída, os três sinais se complementam:

- **Métricas** mostram que existe desvio e sua dimensão;
- **Traces** mostram onde a experiência se degradou ao atravessar componentes;
- **Logs** ajudam a explicar o evento concreto, desde que tenham contexto e correlação.

A utilidade aparece quando eles se conectam a uma decisão explícita.

Por exemplo: uma elevação de latência após uma mudança não exige automaticamente rollback. Primeiro é preciso comparar a jornada afetada, a taxa de sucesso, a propagação para dependências e o consumo de orçamento de erro. Se o impacto é crescente, atinge usuários e ameaça o objetivo de confiabilidade, interromper a mudança deixa de ser uma reação subjetiva e se torna uma decisão defendível.

Essa é também a base para AIOps responsável: um modelo pode ajudar a correlacionar sinais e recomendar hipóteses, mas não substitui a política que define limites, responsáveis, critério de parada e saída segura.

## Próximo passo seguro

Antes de criar um novo dashboard, escolha uma mudança recorrente ou de risco conhecido e responda:

- qual sinal mostra impacto real no usuário;
- qual alteração precisa aparecer na linha do tempo;
- qual combinação de evidências interrompe o avanço;
- e quem decide entre seguir, pausar ou voltar.

Se essas respostas não existem, o problema não é falta de painel. É falta de um contrato operacional para decidir sob incerteza.

---

## Referências

- [Google SRE Workbook — Monitoring Systems with Advanced Analytics](https://sre.google/workbook/monitoring/)
- [Google SRE Workbook — Implementing SLOs](https://sre.google/workbook/implementing-slos/)
- [OpenTelemetry — Unified observability signals](https://opentelemetry.io/)
