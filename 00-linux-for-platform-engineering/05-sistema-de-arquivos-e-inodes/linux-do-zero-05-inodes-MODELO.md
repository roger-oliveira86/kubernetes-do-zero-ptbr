# Linux do Zero #5 — Sistema de arquivos e inodes

> **Modelo de rascunho — não publicar assim.** Escrevi esta versão como ponto de partida, seguindo o mesmo formato dos capítulos 01–04 (conceito → comando → o que você vê → conexão com Kubernetes). Troque os exemplos genéricos abaixo pelos seus próprios testes e capturas de tela/terminal antes de publicar. Onde eu não tinha o dado real, deixei `[SEU EXEMPLO AQUI]`.

## O que você leva deste capítulo

Por que um disco pode estar "cheio" com espaço livre sobrando, e como isso vira `DiskPressure` num nó do Kubernetes.

## O problema que a maioria só entende depois de apanhar

Todo mundo aprende que `df -h` mostra o espaço em disco. Poucos aprendem que existe um segundo limite, independente do espaço em bytes: o número de **inodes**. Um sistema de arquivos reserva, na criação, uma quantidade fixa de inodes — e cada arquivo, por menor que seja, consome exatamente um. Um diretório com milhões de arquivos pequenos (logs, sessões, cache) pode esgotar os inodes com o disco ainda mostrando gigabytes livres.

O sintoma clássico: `No space left on device` ao tentar criar um arquivo, enquanto `df -h` diz que sobram 40 GB.

## Inode: o que ele guarda de fato

Um inode não guarda o nome do arquivo. Ele guarda os metadados: dono, permissões, timestamps, tamanho e os ponteiros para os blocos de dados no disco. O nome do arquivo vive só na entrada do diretório, que aponta para um número de inode. É por isso que renomear um arquivo é instantâneo (só muda a entrada do diretório) e por isso que um `hard link` é, na prática, dois nomes apontando para o mesmo inode.

```
$ ls -i /etc/hostname
131074 /etc/hostname
```

O número `131074` é o inode. Se você criar um hard link para o mesmo arquivo, o `ls -i` mostra o mesmo número.

## Vendo o limite de inodes

```
$ df -i /
Filesystem     Inodes  IUsed   IFree IUse% Mounted on
/dev/sda1     6553600 6531200   22400   99% /
```

`IUse% 99%` com `df -h` mostrando espaço livre é o sinal de que o problema é inode, não byte. [SEU EXEMPLO AQUI: rode em uma VM e cole a saída real, idealmente mostrando os dois comandos lado a lado — `df -h` "saudável" e `df -i` "no limite".]

## Achando quem está consumindo os inodes

```
$ for dir in /var/log /var/cache /tmp; do
    echo "$dir: $(find "$dir" -xdev | wc -l) arquivos"
  done
```

Em produção, os suspeitos de sempre são: diretórios de sessão de aplicações web, filas de e-mail mortas, e logs rotacionados sem limite de retenção. [SEU EXEMPLO AQUI: qual foi o diretório real que você viu estourar — sanitizado, sem nome de sistema ou cliente.]

## `lsof +L1` — o outro jeito de sumir com espaço

Existe uma segunda forma clássica de "disco cheio com espaço livre": um processo mantém aberto um arquivo que já foi deletado. O `unlink` remove a entrada do diretório, mas o inode só é liberado quando o último processo fecha o descritor. Enquanto isso, o espaço não aparece em `du`, mas também não volta para o `df`.

```
$ lsof +L1
COMMAND    PID   USER   FD   TYPE DEVICE SIZE/OFF NLINK   NODE NAME
java      2231   app    12w   REG  8,1   4294967296   0  131099 /var/log/app.log (deleted)
```

A coluna `NLINK 0` é o alerta: zero links no diretório, mas o arquivo ainda ocupa espaço porque algum processo (`PID 2231`) o mantém aberto. A correção não é apagar de novo — já não existe entrada para apagar — é reiniciar ou sinalizar o processo para que ele feche e reabra o arquivo (`logrotate` com `copytruncate`, ou um `kill -HUP`).

## A conexão com o Kubernetes: `DiskPressure`

O kubelet monitora o disco do nó em dois eixos, não um: bytes livres e inodes livres. Quando qualquer um dos dois cruza o limite configurado (`nodefs.inodesFree`, por padrão algo como 5%), o kubelet marca o nó com a condição `DiskPressure` e começa a despejar (`evict`) pods para liberar espaço — começando pelos de menor prioridade, depois os que mais consomem no `emptyDir` e em logs de container.

```
$ kubectl describe node <nome-do-no> | grep -A2 DiskPressure
```

Um nó pode estar com `DiskPressure=True` e `df -h` "normal" ao mesmo tempo — se a causa for inodes esgotados por uma aplicação que gera muitos arquivos pequenos em `/var/lib/containerd` ou no volume de logs. Investigar sem saber disso leva à conclusão errada ("tem espaço, não é disco") e à causa raiz nunca é encontrada.

## O que fazer com isso

1. Ao investigar espaço em disco, rode `df -h` **e** `df -i` sempre juntos. Nunca um sem o outro.
2. Se o volume tem muitos arquivos pequenos e efêmeros (sessões, cache, filas), pergunte antes se ele deveria estar num sistema de arquivos com mais inodes reservados, ou se a aplicação deveria limpar depois de si.
3. Em produção, monitore `node_filesystem_files_free` (Prometheus/node_exporter) com o mesmo alerta que você já tem para bytes livres. Se seu painel só olha bytes, ele está cego para metade do problema.

## Exercício

Numa VM descartável, crie 200 mil arquivos vazios em um diretório de teste e observe o `df -i` mudar enquanto o `df -h` mal se move. Depois, delete o diretório e rode `lsof +L1` propositalmente segurando um arquivo aberto num terminal enquanto apaga em outro, para ver o `NLINK 0` na prática.

---

*Este capítulo faz parte da trilha [Linux para Platform Engineering](../README.md).*
