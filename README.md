<h2>Configuração Servidor de Logs</h2>

<h3>Servidor</h3>

aluno@ubuntu:~$ sudo apt update<br>
[sudo] senha para aluno: <br>
Obter:1 http://br.archive.ubuntu.com/ubuntu focal InRelease [265 kB]<br>
Obter:2 http://br.archive.ubuntu.com/ubuntu focal-updates InRelease [111 kB]<br>
Obter:3 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]<br>
Lendo listas de pacotes... Pronto<br><br>

aluno@ubuntu:~$ sudo apt install rsyslog<br>
Lendo listas de pacotes... Pronto<br>
Construindo árvore de dependências<br>       
Lendo informação de estado... Pronto<br>
rsyslog já é a versão mais nova (8.2001.0-1ubuntu1).<br>
0 pacotes atualizados, 0 pacotes novos instalados, 0 a serem removidos.<br><br>

aluno@ubuntu:~$ sudo nano /etc/rsyslog.conf<br>
(usuário edita e altera os seguintes trechos do arquivo)<br><br>

module(load="imudp")<br>
input(type="imudp" port="514")<br><br>

module(load="imtcp")<br>
input(type="imtcp" port="514")<br><br>

(Ctrl+O para salvar, Enter, Ctrl+X para sair)<br><br>

aluno@ubuntu:~$ sudo systemctl restart rsyslog<br><br>

aluno@ubuntu:~$ sudo systemctl status rsyslog<br>
● rsyslog.service - System Logging Service<br>
     Loaded: loaded (/lib/systemd/system/rsyslog.service; enabled; vendor preset: enabled)<br>
     Active: active (running) since Mon 2025-04-29 18:05:17 -03; 6s ago<br>
TriggeredBy: ● syslog.socket<br>
   Main PID: 1298 (rsyslogd)<br>
      Tasks: 4 (limit: 4695)<br>
     Memory: 2.3M<br>
     CGroup: /system.slice/rsyslog.service<br>
             └─1298 /usr/sbin/rsyslogd -n -iNONE<br>
abr 29 18:05:17 ubuntu rsyslogd[1298]: [origin software="rsyslogd" swVersion="8.2001.0" x-pid="1298" x-info="https://www.rsyslog.com"] start<br><br>

aluno@ubuntu:~$ sudo tail -f /var/log/syslog<br>
Apr 29 18:06:30 client1 sshd[2134]: Accepted password for pedro from 192.168.1.21 port 50832 ssh2<br>
Apr 29 18:06:31 client1 sudo:    pedro : TTY=pts/0 ; PWD=/home/pedro ; USER=root ; COMMAND=/bin/ls<br>
Apr 29 18:06:32 client1 systemd[1]: Starting Daily apt download activities...<br>
Apr 29 18:06:33 client1 systemd[1]: Finished Daily apt download activities.<br>
Apr 29 18:06:35 client1 sudo:    pedro : TTY=pts/0 ; PWD=/home/pedro ; USER=root ; COMMAND=/bin/cat /etc/passwd<br><br>


<h3>Cliente</h3>

aluno@cliente1:~$ sudo apt update<br>
[sudo] senha para aluno: <br>
Obter:1 http://br.archive.ubuntu.com/ubuntu focal InRelease [265 kB]<br>
Obter:2 http://br.archive.ubuntu.com/ubuntu focal-updates InRelease [111 kB]<br>
Obter:3 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]<br>
Lendo listas de pacotes... Pronto<br><br>

aluno@cliente1:~$ sudo apt install rsyslog<br>
Lendo listas de pacotes... Pronto<br>
Construindo árvore de dependências      <br> 
Lendo informação de estado... Pronto<br>
rsyslog já é a versão mais nova (8.2001.0-1ubuntu1).<br>
0 pacotes atualizados, 0 pacotes novos instalados, 0 a serem removidos.<br><br>

aluno@cliente1:~$ sudo nano /etc/rsyslog.d/remote.conf<br>
(usuário insere a linha abaixo no novo arquivo)<br><br>

*.*  @@192.168.1.10:514<br><br>

(Ctrl+O para salvar, Enter, Ctrl+X para sair)<br><br>

aluno@cliente1:~$ sudo systemctl restart rsyslog<br><br>

aluno@cliente1:~$ sudo systemctl status rsyslog<br><br>
● rsyslog.service - System Logging Service<br>
     Loaded: loaded (/lib/systemd/system/rsyslog.service; enabled; vendor preset: enabled)<br>
     Active: active (running) since Mon 2025-04-29 18:07:52 -03; 5s ago<br>
TriggeredBy: ● syslog.socket<br>
   Main PID: 1356 (rsyslogd)<br>
      Tasks: 4 (limit: 4705)<br>
     Memory: 2.2M<br>
     CGroup: /system.slice/rsyslog.service<br>
             └─1356 /usr/sbin/rsyslogd -n -iNONE<br>
abr 29 18:07:52 cliente1 rsyslogd[1356]: [origin software="rsyslogd" swVersion="8.2001.0" x-pid="1356" x-info="https://www.rsyslog.com"] start<br><br>

aluno@cliente1:~$ logger "Log de teste enviado ao servidor de logs"<br><br>

<h3>Servidor</h3>

Apr 29 18:08:01 cliente1 aluno: Log de teste enviado ao servidor de logs<br><br>
