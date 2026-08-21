# Linux do Zero #2 — Processos: o que roda por baixo do seu terminal

*Parte 2 da série "Linux do Zero". Se você ainda não leu a #1, ela está no Medium e no DEV.to — comece por lá para entender a proposta: investigar por evidências, não decorar comando.*

## O problema que ninguém explica direito

Você já rodou `ps aux` e viu uma lista enorme de linhas passar na tela, sem saber muito bem o que fazer com aquilo? A maioria dos tutoriais te ensina a rodar o comando, mas não te ensina a *pensar* em cima do resultado. Esse post é sobre isso: como transformar uma lista de números em um raciocínio sobre o que a sua máquina está realmente fazendo.

## O experimento

Abra dois terminais lado a lado. No primeiro, rode:

```bash
sleep 300 &
```

Isso cria um processo que só fica "dormindo" por 300 segundos — sem fazer nada de útil, de propósito, para servirmos de cobaia.

No segundo terminal, rode:

```bash
ps -eo pid,ppid,stat,etime,cmd | grep sleep
```

Você vai ver uma linha parecida com esta:

```
  PID   PPID STAT     ELAPSED CMD
12345   9876 S           0:03 sleep 300
```

Agora a pergunta que importa: **o que cada uma dessas colunas está te dizendo sobre a vida desse processo?**

- `PID` — a identidade única desse processo enquanto ele existir.
- `PPID` — quem criou esse processo (o processo pai). No seu caso, provavelmente o próprio shell do primeiro terminal.
- `STAT` — o estado atual. `S` significa "sleeping" (interrompível). Você vai ver `R` (rodando), `Z` (zumbi) e `D` (esperando I/O não interrompível) em outros contextos — cada um conta uma história diferente sobre o que está travando ou não o sistema.
- `ETIME` — há quanto tempo ele existe.

## Por que isso importa de verdade

Em produção, ninguém roda `sleep` de propósito — mas todo incidente de performance que já investiguei começou exatamente com essa pergunta: "quais processos estão rodando, quem é o pai de quem, e em que estado eles estão parados?". Um processo em estado `D` (uninterruptible sleep) por muito tempo, por exemplo, é quase sempre sintoma de um disco ou storage sofrendo — não de aplicação com bug.

Tente agora matar o processo pelo pai, não pelo `sleep` diretamente:

```bash
kill -TERM <PPID_do_seu_shell>
```

**Não rode isso de verdade no seu terminal principal** — é só para você visualizar mentalmente o que aconteceria: ao matar o processo pai, o `sleep` filho normalmente também morre (ou é "adotado" pelo `init`/`systemd`, dependendo do sistema). Esse comportamento de reparentamento é a mesma lógica por trás de por que, quando um container morre inesperadamente, os processos que ele hospedava desaparecem juntos.

## A ponte para onde essa trilha está indo

Essa relação pai/filho de processos é exatamente o que o Linux usa como base para isolar processos em namespaces — o mecanismo que, mais adiante nesta trilha, vira containers, e que o Kubernetes orquestra em escala. Entender processo, hoje, sem pressa, é o que faz o "aha" acontecer quando você chegar em `cgroups` e `namespaces` daqui a duas ou três postagens.

## Para você investigar por conta própria

Antes do próximo post, tente responder isto na sua própria máquina (não me responda aqui, é para você mesmo):

1. Rode `ps -eo pid,ppid,stat,etime,cmd --forest` e identifique visualmente qual processo é pai de qual.
2. Ache um processo em estado `Z` (zumbi) — se não tiver nenhum, isso já é uma informação: seu sistema está saudável nesse quesito agora.
3. Compare `ps aux` com `top` (ou `htop`) rodando ao mesmo tempo — o que muda entre uma foto estática e uma visão contínua?

---
*Próximo da trilha: namespaces e o que realmente isola um processo do outro — a base de tudo que chamamos hoje de "container".*
