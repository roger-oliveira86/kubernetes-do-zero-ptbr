# Modelo de cenário executável

Use este modelo para criar laboratórios em que o leitor aprende a pensar operacionalmente, não apenas a repetir comandos.

## Cenário

Descreva uma situação concreta em uma frase. Exemplo: “A aplicação está saudável, mas o Service não possui endpoints”.

## Pergunta de investigação

Qual pergunta o leitor precisa responder antes de agir?

> O selector do Service corresponde às labels dos Pods prontos?

## Objetivo e definição de pronto

Declare o estado verificável ao final do exercício.

- [ ] recurso criado no namespace do laboratório;
- [ ] sinal esperado observado;
- [ ] falha reproduzida apenas no ambiente descartável;
- [ ] causa identificada com evidência;
- [ ] estado saudável restaurado;
- [ ] recursos removidos.

## Ambiente seguro

Indique uma opção no navegador, uma alternativa local e o comando de limpeza.

| Opção | Uso | Limpeza |
| --- | --- | --- |
| Killercoda | laboratório no navegador | encerrar a sessão |
| kind | cluster local temporário | `kind delete cluster --name <nome>` |
| minikube | cluster local guiado | `minikube delete` |

Inclua sempre:

```bash
kubectl config current-context
kubectl version
```

Se o contexto não for descartável, o leitor deve parar.

## Estado inicial

Liste os manifestos e explique a função de cada um. Em seguida, forneça os comandos de aplicação e os sinais que confirmam o estado saudável.

## Mudança controlada

Apresente uma mudança pequena e reversível. Explique:

- o que muda no estado desejado;
- qual controlador deve reagir;
- qual evidência demonstra progresso.

## Falha intencional

A falha deve ser segura, previsível e restrita ao namespace de laboratório. Não use dados reais, credenciais ou recursos compartilhados.

## Investigação por evidência

Organize a investigação em perguntas, não em tentativa e erro:

1. qual recurso deveria produzir o comportamento?
2. qual sinal mostra o impacto?
3. qual evento, log ou condição explica a causa?
4. qual é a menor correção ou saída segura?

Inclua apenas os comandos necessários.

## Recuperação e validação

Explique se a saída correta é corrigir, pausar ou fazer rollback. Depois, mostre como comprovar que o estado saudável foi restaurado.

## Limpeza

Remova o namespace ou recursos criados e peça uma última confirmação.

## Perguntas de revisão

Faça perguntas que obriguem o leitor a justificar uma decisão. Evite perguntas que peçam apenas a sintaxe de um comando.

## Referências

Use documentação oficial para cada objeto e comando central do cenário.
