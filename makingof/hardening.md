# Hardening

Nossos métodos de Hardening do ambiente dos desafios de Infraestrutura [`Cyber`]


[`custombashrc`](https://github.com/insidersec/ctf_writeups/blob/master/makingof/hardening/custombashrc)
---

### Arquivo .bashrc customizado

linha 16:`#shopt -s histappend`
>Comentando o `shopt -s histappend` garantimos que o history sempre sobrescreva e não acumule os históricos de sessão

linha 21: `#HISTFILE=`
>Comentando o `HISTFILE` garantimos que o histórico da sessão não seja salvo, portanto o comando `history` não mostre os comandos executados pelos outros jogadores 

linha 22: umask 007
>Utilizando o `umask 007` garantimos que os participantes criem apenas arquivos para o próprio user, no padrão UNIX `UGO` User Group Others, a máscara funciona ao contrário das permissões, se queremos uma permissão 660 utilizamos a máscara 007. A permissão 6 é atribuída por padrão na criação dos arquivos para que nada tenha a permissão `x` de execução quando criado. Precisa que seja executado `chmod +x` ou `chmod 777` (O 777 não é recomendado pois libera para que todos os usuários da máquina possam ler, escrever e executar esse arquivo ou pasta). 

linha 117-126: Banner Insidersec


[`setuserperm.sh`](https://github.com/insidersec/ctf_writeups/blob/master/makingof/hardening/setuserperm.sh)
---

### Script executado para executar o hardening do servidor (precisa ser executado com `sudo`)

linha 4-5:
>Executa um loop em todos usuários do sistema listados no `/etc/passwd` filtrando por `cyber` e cortando para retornar apenas o nome do usuário.

linha 6:
>Retorna o valor da varíavel `i`, para acompanhar o desenrolar do script e listar os usuários que foram aplicados as modificações.

linha7:
>Sobrescreve o `.bashrc` de cada usuário pelo `custombashrc`

linha8:
>Vai até a `Home`de cada user

linha9:
>Seta o o dono do arquivo como `root` para que as regras de leitura e gravação sejam reforçadas e o usuário não possa editar os arquivos nem usando `force` e também usa a variável `$i` para que o user faça parte do `Group` de permissão. Esse comando todo basicamente garante que a máscara aplicada nos arquivos e pastas sejam efetivas.

linha10:
>Faz o mesmo para o diretório `/home/$i`

linha11:
>Aplica o permissionamento `050` que permite que Users do `Group` autorizado possam ler e executar (não podem escrever) os arquivos `.bash*` `.profile` `.viminfo`

linha12:
>Aplica o permissionamento `050` no diretório `/home/$i`

recomendo: [Permissions Calculator](http://permissions-calculator.org)

[`crontab`](https://github.com/insidersec/ctf_writeups/blob/master/makingof/hardening/crontab)
---

### Nossas tarefas agendadas

O crontab do `root` pode ser acessado com `sudo crontab -e`

```
# m h  dom mon dow   command
  0 * * * * touch /home/admin/cron_rodou     #Toca o arquivo /home/admin/cron_rodou atualizando o horário de alteração do arquivo para verificar se o cron rodou
  0 * * * * rm -rf /tmp/*                    # apaga todo o conteúdo da pasta temporária /tmp a cada minuto 0, portanto a cada hora redonda 1:00, 2:00, 3:00...
```
recomendo:  [Crontab Guru](https://crontab.guru/#0_*_*_*_*)


---

[Voltar ao Menu Inicial](https://writeup.insidersec.io/)