# Migração em ondas com critérios de avanço e rollback

Este laboratório demonstra uma estratégia segura para realizar mudanças progressivas em uma aplicação Kubernetes.

## Objetivo

Executar uma migração em ondas observando:

* taxa de erro;
* latência;
* disponibilidade;
* comportamento do tráfego;
* critérios para avançar;
* critérios para interromper ou realizar rollback.

## Cenário

A aplicação começa com uma versão estável. Uma nova versão é introduzida para uma parcela pequena do tráfego. A onda seguinte somente é liberada quando os indicadores permanecem dentro dos limites definidos.

## Critérios de avanço

Antes de avançar para a próxima onda:

* erros permanecem abaixo do limite definido;
* latência não apresenta degradação relevante;
* não há saturação de CPU ou memória;
* logs e eventos não indicam falhas novas;
* o procedimento de rollback foi validado.

## Critérios de rollback

A mudança deve ser interrompida quando ocorrer:

* aumento sustentado de erros;
* degradação significativa da latência;
* falha de readiness ou liveness;
* saturação de recursos;
* impacto funcional identificado pelo usuário.

O objetivo não é eliminar todo risco. É tornar o risco observável, limitar o impacto e manter uma decisão reversível.

## Pergunta para o leitor

Quais indicadores sua equipe usaria para decidir entre avançar, pausar ou fazer rollback?

