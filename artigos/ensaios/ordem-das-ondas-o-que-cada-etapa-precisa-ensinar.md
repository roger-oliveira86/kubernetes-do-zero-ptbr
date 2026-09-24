# Uma decisão #3 — A ordem das ondas (modelo genérico, para ensinar, não para expor)

> **Como usar este arquivo:** isto é um modelo de ensino — a estrutura e o raciocínio são reais, os detalhes são inventados e genéricos de propósito, para não expor a empresa atual. Ajuste o roteiro do meio (as ondas e os critérios) com a lógica real que você usou, mas mantendo nomes, sistemas e números fictícios ou arredondados, exatamente como fizemos nos ensaios já publicados.

---

## Rascunho para o post do LinkedIn (5–8 min de leitura)

**A ordem das ondas não é sobre o que é mais fácil migrar primeiro. É sobre o que ensina mais rápido, com menos gente por perto se algo der errado.**

Quando o plano de migração ficou pronto, a primeira proposta que apareceu foi a intuitiva: começar pelo que tem menos dependências, terminar pelo que tem mais. Faz sentido no papel. Na prática, essa ordem otimiza para "terminar rápido as fáceis" e deixa o time menos preparado exatamente quando o risco é maior — no fim, com as peças mais críticas.

A ordem que usamos seguiu outro critério: **cada onda precisa ensinar algo que a próxima onda vai precisar.**

- **Onda 1 — baixo risco, alta visibilidade de erro.** Um serviço sem tráfego real de usuário, mas com todas as integrações de rede, autenticação e observabilidade que os demais também têm. Se algo quebrasse aqui, quebrava sem cliente no meio, e a causa seria genérica o bastante para valer para o resto do parque. Foi aqui que descobrimos, por exemplo, que uma regra de firewall estava documentada errado — melhor descobrir na onda 1 que na 6.
- **Onda 2 — primeiro serviço com tráfego real, mas de baixa criticidade de negócio.** Testava o rollback com usuário de verdade, sem ser um usuário que dói perder. Foi a primeira vez que o critério de rollback (definido antes de qualquer onda começar) foi realmente exercitado sob pressão, e não só ensaiado em laboratório.
- **Ondas 3 e 4 — grupos por padrão de acesso, não por dono.** Agrupamos por como o serviço se comportava (leitura pesada, escrita pesada, dependente de fila), não por qual time era dono. Isso trouxe atrito — dois times que nunca tinham migrado junto tiveram que coordenar uma janela — mas evitou que "o jeito como o time X faz deploy" virasse uma exceção não testada nas ondas seguintes.
- **Onda 5 — o serviço que dependia de autenticação centralizada.** Foi deliberadamente colocado no meio, não no fim. A tentação natural é deixar "o mais complicado" por último, quando o time já está cansado. Fizemos o oposto: colocamos a autenticação assim que tínhamos confiança suficiente no rollback, mas ainda com energia de sobra para lidar com o imprevisto — e imprevisto houve.
- **Onda 6 (última) — o conjunto de maior criticidade de negócio.** Só chegou à última posição depois que todas as classes de falha possíveis já tinham aparecido, uma vez, em algo menos crítico. Nada nessa onda foi, tecnicamente, novidade.

**O que isso custou:** a onda 5 (autenticação) atrasou duas semanas porque um caso de borda só apareceu ali — apesar de estar no meio do plano, não no fim. Migrar por "o que ensina mais" significa aceitar que a curva de aprendizado dói no meio do caminho, não só no início. A alternativa — deixar tudo que é arriscado para o fim — parece mais segura, mas só empurra a dor para quando o time está mais cansado e com menos margem para errar.

**A pergunta que fica:** se você está planejando uma sequência de mudanças arriscadas, você está ordenando pelo que é mais fácil terminar primeiro, ou pelo que ensina mais rápido o que as próximas etapas vão precisar saber?

---

## Estrutura genérica, para reaproveitar em qualquer decisão de ordenação

Se quiser reescrever isso com os detalhes reais da sua migração (mantendo tudo sanitizado, como nos outros ensaios), o roteiro por trás do texto é:

1. **A ordem óbvia e por que ela falha.** (menor dependência → maior dependência, ou "o que o cliente pede primeiro")
2. **O critério real usado.** (o que cada etapa precisa ensinar para a próxima)
3. **3 a 5 ondas**, cada uma com: o que era, por que entrou naquela posição, o que ela ensinou.
4. **Uma onda que custou mais do que o esperado**, e por quê — sem culpar pessoa ou time.
5. **Fechar em pergunta para o leitor**, nunca em lição de moral.

As mesmas três regras da série continuam valendo: situação concreta e verificável, nunca nomear o empregador nem sugerir negligência alheia, e terminar no custo, não na lição.
