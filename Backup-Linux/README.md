<h1>BackupSH Linux</h1>

<p>
Este programa tem por objetivo realizar backups locais das aplicações e baco de dados, compactando, armazenando em um diretório local e posteriormente levando estes arquivos a um servidor remoto.
</p>

<p>
O procedimento para instalação é simples. Em um servidor linux, basta baixar o script e adicionar a rotina no cront do sistema operacional. Procedimento:
</p>

<pre>
cd /opt/
git clone https://gitlab.enap.gov.br/cgti-infra/backupsh.git

cp backupsh/backupsh.conf.dist backupsh/backupsh.conf
chown -R root. backupsh/
chmod 700 backupsh/
chmod 600 backupsh/*
chmod 700 backupsh/backupsh.sh
</pre>

<p>
Editar e ajustar o arquivo <code>backupsh.conf</code> conforme as necessidades de cada servidor. Depois de ajustar as configurações, executar o script e certificar de que não houve saída de erro.
</p>

Adicionar a rotina de backup no CRON <code>/etc/crontab</code>

<pre>
# Backup
40 22	* * *	root	cd /opt/backupsh/; git pull
00 23   * * *   root	bash /opt/backupsh/backupsh.sh
</pre>

<p>
Adicionar a rotina de rotação dos log's no sistema operacional <code>/etc/logrotate.d/backupsh</code>.
</p>

<pre>
/var/log/backupsh*.log {
   rotate 5
   weekly
   missingok
   notifempty
   copytruncate
   compress
}
</pre>