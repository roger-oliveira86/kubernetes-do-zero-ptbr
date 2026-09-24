# 05 — Sistema de arquivos e inodes

> Parte da trilha **Linux do Zero** (peça #5: *Sistema de arquivos e inodes*).

## Objetivo

Entender por que um disco pode estar "cheio" com espaço em bytes sobrando — e como esse mesmo limite, quando ignorado, vira `DiskPressure` num nó do Kubernetes.

Pré-requisito: terminal básico (peça #1) e usuários/permissões (peça #4) desta trilha.

## Estrutura deste laboratório

```text
05-sistema-de-arquivos-e-inodes/
└── README.md   # este arquivo
```

## 1. O problema que `df -h` não mostra

Todo mundo aprende que `df -h` mostra o espaço em disco. Poucos aprendem que existe um segundo limite, independente do espaço em bytes: o número de **inodes**. Um sistema de arquivos reserva, na criação, uma quantidade fixa de inodes — e cada arquivo, por menor que seja, consome exatamente um. Um diretório com milhões de arquivos pequenos (logs, sessões, cache) pode esgotar os inodes com o disco ainda mostrando gigabytes livres.

O sintoma clássico: `No space left on device` ao tentar criar um arquivo, enquanto `df -h` diz que sobram dezenas de gigabytes.

## 2. O que um inode guarda de fato

Um inode não guarda o nome do arquivo. Ele guarda os metadados: dono, permissões, timestamps, tamanho e os ponteiros para os blocos de dados no disco. O nome do arquivo vive só na entrada do diretório, que aponta para um número de inode. É por isso que renomear um arquivo é instantâneo (só muda a entrada do diretório), e por isso que um `hard link` é, na prática, dois nomes apontando para o mesmo inode.

```bash
ls -i /etc/hostname
```

**Exercício:** rode o comando acima e anote o número. Crie um hard link (`ln /etc/hostname /tmp/hostname-link`) e rode `ls -i` nos dois — o número deve ser idêntico.

## 3. Vendo o limite de inodes

```bash
df -i /
```

Se `IUse%` estiver perto de 100% enquanto `df -h` mostra espaço livre, o problema é inode, não byte.

**Exercício:** compare a saída de `df -h /` e `df -i /` na sua máquina. Depois, crie 200 mil arquivos vazios num diretório de teste (`for i in $(seq 1 200000); do touch /tmp/teste/f$i; done`) e observe o `df -i` mudar enquanto o `df -h` mal se move. Não esqueça de limpar o diretório depois.

## 4. Achando quem consome os inodes

```bash
for dir in /var/log /var/cache /tmp; do
  echo "$dir: $(find "$dir" -xdev | wc -l) arquivos"
done
```

Em produção, os suspeitos de sempre são: diretórios de sessão de aplicações web, filas de e-mail mortas e logs rotacionados sem limite de retenção.

## 5. `lsof +L1` — o outro jeito de sumir com espaço

Existe uma segunda forma clássica de "disco cheio com espaço livre": um processo mantém aberto um arquivo que já foi deletado. O `unlink` remove a entrada do diretório, mas o inode só é liberado quando o último processo fecha o descritor. Enquanto isso, o espaço não aparece em `du`, mas também não volta para o `df`.

```bash
lsof +L1
```

Uma linha com `NLINK 0` é o alerta: zero links no diretório, mas o arquivo ainda ocupa espaço porque algum processo o mantém aberto. A correção não é apagar de novo — já não existe entrada para apagar — é reiniciar ou sinalizar o processo para que feche e reabra o arquivo (`logrotate` com `copytruncate`, ou um `kill -HUP`).

**Exercício:** num terminal, abra um arquivo com `tail -f arquivo.log`; em outro terminal, apague o arquivo (`rm arquivo.log`); rode `lsof +L1` e observe a linha com `NLINK 0` enquanto o primeiro terminal ainda o mantém aberto.

## 6. A conexão com o Kubernetes: `DiskPressure`

O kubelet monitora o disco do nó em dois eixos, não um: bytes livres e inodes livres. Quando qualquer um dos dois cruza o limite configurado (`nodefs.inodesFree`, por padrão perto de 5%), o kubelet marca o nó com a condição `DiskPressure` e começa a despejar (`evict`) pods para liberar espaço — começando pelos de menor prioridade, depois os que mais consomem em `emptyDir` e em logs de container.

```bash
kubectl describe node <nome-do-no> | grep -A2 DiskPressure
```

Um nó pode estar com `DiskPressure=True` e `df -h` "normal" ao mesmo tempo, se a causa for inodes esgotados por uma aplicação que gera muitos arquivos pequenos em `/var/lib/containerd` ou no volume de logs. Investigar sem saber disso leva à conclusão errada ("tem espaço, não é disco"), e a causa raiz nunca é encontrada.

## Checklist de investigação

1. Rode `df -h` **e** `df -i` sempre juntos — nunca um sem o outro.
2. Se o volume tem muitos arquivos pequenos e efêmeros, pergunte se ele deveria ter mais inodes reservados, ou se a aplicação deveria limpar depois de si.
3. Em produção, monitore `node_filesystem_files_free` (Prometheus/node_exporter) com o mesmo alerta que já existe para bytes livres.

Os exemplos são educacionais. Confirme comportamento, versão e permissões no ambiente em que forem executados.

---

*Série Linux do Zero — [Roger Oliveira](https://www.linkedin.com/in/oliveiraroger/). Repositório: [kubernetes-do-zero-ptbr](https://github.com/roger-oliveira86/kubernetes-do-zero-ptbr).*
