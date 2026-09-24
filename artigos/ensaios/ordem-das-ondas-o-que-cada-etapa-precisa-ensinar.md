# A ordem das ondas: o que cada etapa precisa ensinar

Este ensaio discute como ordenar uma sequência de mudanças arriscadas — não pela facilidade de execução, mas pelo que cada etapa ensina para a próxima.

## Objetivo

Discutir um critério de ordenação para migrações e mudanças executadas em múltiplas etapas ("ondas"), observando:

* o que cada onda precisa ensinar para reduzir o risco da próxima;
* por que a ordem "mais fácil primeiro, mais arriscado por último" costuma falhar;
* onde colocar a etapa mais crítica dentro da sequência;
* o custo real de ordenar por aprendizado em vez de por facilidade.

## O critério óbvio, e por que ele falha

A ordem mais intuitiva para uma sequência de mudanças é começar pelo que tem menos dependências e terminar pelo que tem mais. Faz sentido no papel: otimiza para "terminar rápido as fáceis". Na prática, essa ordem deixa o time menos preparado exatamente quando o risco é maior — no fim da sequência, com as peças mais críticas, depois de semanas de execução e com a atenção do time já desgastada.

## O critério usado: cada onda ensina algo que a próxima vai precisar

A alternativa é ordenar por aprendizado, não por facilidade: cada etapa é posicionada para revelar, o mais cedo possível, o tipo de falha que as etapas seguintes poderiam sofrer.

* **Primeira onda — baixo risco, alta visibilidade de erro.** Um componente sem tráfego real de usuário, mas com todas as integrações (rede, autenticação, observabilidade) que os demais também têm. Se algo quebra aqui, quebra sem cliente no meio, e a causa costuma ser genérica o bastante para valer para o restante do escopo.
* **Segunda onda — primeiro caso com tráfego real, de baixa criticidade de negócio.** Testa o critério de rollback com uso real, sem ser um uso que dói perder. É a primeira vez que o critério definido antes de qualquer onda começar é exercitado sob pressão, e não apenas ensaiado em laboratório.
* **Ondas intermediárias — agrupadas por padrão de comportamento, não por dono.** Agrupar por como o componente se comporta (leitura pesada, escrita pesada, dependência de fila), em vez de por qual time é dono, evita que "o jeito como um time específico opera" vire uma exceção nunca testada nas ondas seguintes.
* **A etapa mais crítica — no meio da sequência, não no fim.** A tentação natural é deixar o mais complicado por último, quando o time já está cansado. O oposto costuma funcionar melhor: colocar a etapa mais delicada assim que já existe confiança suficiente no critério de rollback, mas ainda com energia de sobra para lidar com o imprevisto.
* **Última onda — o conjunto de maior criticidade de negócio.** Só chega à última posição depois que todas as classes de falha possíveis já apareceram, uma vez, em algo menos crítico.

## O custo real

Ordenar por "o que ensina mais" tem um preço: a curva de aprendizado dói no meio do caminho, não só no início. Uma etapa posicionada deliberadamente no meio da sequência pode atrasar mais do que o esperado, justamente porque um caso de borda só aparece ali. A alternativa — deixar tudo que é arriscado para o fim — parece mais segura no papel, mas só empurra a dificuldade para quando o time está mais cansado e com menos margem para errar.

## A pergunta que fica

Ao planejar uma sequência de mudanças arriscadas, a pergunta que vale fazer é: a ordem está otimizada para terminar rápido o que é fácil, ou para aprender rápido o que as próximas etapas vão precisar saber?

---

*Este ensaio usa uma situação genérica, sem identificar sistema, empresa ou incidente real.*
