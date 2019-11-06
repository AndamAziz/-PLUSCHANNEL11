#!/bin/bash
echo "$ipdovps" >/etc/IP
rm -rf /etc/SSHPlus/bot /etc/SSHPlus/ShellBot.sh /etc/SSHPlus/cabecalho /etc/SSHPlus/proxy.py > /dev/null 2>&1
echo "America/Sao_Paulo" > /etc/timezone
echo 'crazy' > /usr/lib/sshplus
ln -fs /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime > /dev/null 2>&1
dpkg-reconfigure --frontend noninteractive tzdata > /dev/null 2>&1
[[ ! -d /etc/SSHPlus ]] && mkdir /etc/SSHPlus
[[ ! -d /etc/SSHPlus/senha ]] && mkdir /etc/SSHPlus/senha
[[ ! -e /etc/SSHPlus/Exp ]] && touch /etc/SSHPlus/Exp
[[ ! -d /etc/SSHPlus/userteste ]] && mkdir /etc/SSHPlus/userteste
if ps x | grep "bot_plus"|grep -v grep 1>/dev/null 2>/dev/null; then
	pidbot_plus=$(ps x|grep "bot_plus"|awk -F "pts" {'print $1'})
    kill -9 $pidbot_plus > /dev/null 2>&1
    screen -wipe > /dev/null 2>&1
fi
if [[ -e /etc/squid3/squid.conf ]]; then
wget https://www.dropbox.com/s/l90w4ip7afyn9hp/squid1.txt -O /tmp/sqd1 1>/dev/null 2>/dev/null
echo "acl url3 dstdomain -i $ipdovps" > /tmp/sqd2
wget https://www.dropbox.com/s/egxvi41jdoxp94x/squid2.txt -O /tmp/sqd3 1>/dev/null 2>/dev/null
cat /tmp/sqd1 /tmp/sqd2 /tmp/sqd3 > /etc/squid3/squid.conf
wget https://www.dropbox.com/s/3ca1urtlenfpxnw/payload.txt -O /etc/squid3/payload.txt 1>/dev/null 2>/dev/null
echo " " >> /etc/squid3/payload.txt
elif [[ -e /etc/squid/squid.conf ]]; then
wget https://www.dropbox.com/s/l90w4ip7afyn9hp/squid1.txt -O /tmp/sqd1 1>/dev/null 2>/dev/null
echo "acl url3 dstdomain -i $ipdovps" > /tmp/sqd2
wget https://www.dropbox.com/s/5esfgr21p0lwqj0/squid.txt -O /tmp/sqd3 1>/dev/null 2>/dev/null
cat /tmp/sqd1 /tmp/sqd2 /tmp/sqd3 > /etc/squid/squid.conf
wget https://www.dropbox.com/s/3ca1urtlenfpxnw/payload.txt -O /etc/squid/payload.txt 1>/dev/null 2>/dev/null
echo " " >> /etc/squid/payload.txt
fi
fun_sqdconf () {
if [[ -d "/etc/squid/" ]]; then
	var_sqd="/etc/squid/squid.conf"
	var_pay="/etc/squid/payload.txt"
elif [[ -d "/etc/squid3/" ]]; then
	var_sqd="/etc/squid3/squid.conf"
	var_pay="/etc/squid3/payload.txt"
else
	apt-get autoremove -y && apt-get autoclean -y
	var_sqd="/etc/squid3/squid.conf"
	var_pay="/etc/squid3/payload.txt"
fi
echo ".claro.com.br/
.claro.com.sv/
.facebook.net/
.netclaro.com.br/
.speedtest.net/
.tim.com.br/
.vivo.com.br/
.oi.com.br/" > $var_pay
echo "acl url1 dstdomain -i 127.0.0.1
acl url2 dstdomain -i localhost
acl url3 dstdomain -i $ipdovps
acl url4 dstdomain -i /SSHPLUS?
acl url5 dstdomain -i /VPSPLUS?
acl payload url_regex -i "$var_pay"
acl all src 0.0.0.0/0
http_access allow url1
http_access allow url2
http_access allow url3
http_access allow url4
http_access allow payload
http_access deny all
 
#Portas
http_port 80
http_port 8080
http_port 8799
visible_hostname SSHPLUS
 
via off
forwarded_for off
pipeline_prefetch off" > $var_sqd
}
netstat -nplt| grep -w 'apache2' |grep -w '80' && apt remove apache2 -y
[[ "$(grep -o '#Port 22' /etc/ssh/sshd_config)" == "#Port 22" ]] && sed -i "s;#Port 22;Port 22;" /etc/ssh/sshd_config && service ssh restart
grep -v "^PasswordAuthentication" /etc/ssh/sshd_config > /tmp/passlogin && mv /tmp/passlogin /etc/ssh/sshd_config
echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config

wget https://unprepossessing-amo.000webhostapp.com/addhost.sh -O /bin/addhost > /dev/null 2>&1
chmod +x /bin/addhost
wget https://unprepossessing-amo.000webhostapp.com/verifatt.sh -O /bin/verifatt > /dev/null 2>&1
chmod +x /bin/verifatt
wget https://unprepossessing-amo.000webhostapp.com/verifbot.sh -O /bin/verifbot > /dev/null 2>&1
chmod +x /bin/verifbot
wget https://unprepossessing-amo.000webhostapp.com/alterarsenha.sh -O /bin/alterarsenha > /dev/null 2>&1
chmod +x /bin/alterarsenha
wget https://raw.githubusercontent.com/chinwi88/dzz/master/criarusuario.sh -O /bin/criarusuario > /dev/null 2>&1
chmod +x /bin/criarusuario
wget https://unprepossessing-amo.000webhostapp.com/delhost.sh -O /bin/delhost > /dev/null 2>&1
chmod +x /bin/delhost
wget https://unprepossessing-amo.000webhostapp.com/expcleaner.sh -O /bin/expcleaner > /dev/null 2>&1
chmod +x /bin/expcleaner
wget https://unprepossessing-amo.000webhostapp.com/mudardata.sh -O /bin/mudardata > /dev/null 2>&1
chmod +x /bin/mudardata
wget https://raw.githubusercontent.com/chinwi88/dzz/master/criarteste.sh -O /bin/criarteste > /dev/null 2>&1
chmod +x /bin/criarteste
wget https://unprepossessing-amo.000webhostapp.com/remover.sh -O /bin/remover > /dev/null 2>&1
chmod +x /bin/remover
wget https://unprepossessing-amo.000webhostapp.com/alterarlimite.sh -O /bin/alterarlimite > /dev/null 2>&1
chmod +x /bin/alterarlimite
wget https://unprepossessing-amo.000webhostapp.com/ajuda.sh -O /bin/ajuda > /dev/null 2>&1
chmod +x /bin/ajuda
wget https://raw.githubusercontent.com/chinwi88/dzz/master/sshmonitor.sh -O /bin/sshmonitor > /dev/null 2>&1
chmod +x /bin/sshmonitor
wget https://unprepossessing-amo.000webhostapp.com/badvpn.sh -O /bin/badvpn > /dev/null 2>&1
chmod +x /bin/badvpn
wget https://unprepossessing-amo.000webhostapp.com/userbackup.sh -O /bin/userbackup > /dev/null 2>&1
chmod +x /bin/userbackup
wget https://unprepossessing-amo.000webhostapp.com/blockt.sh -O /bin/blockt > /dev/null 2>&1
chmod +x /bin/blockt
wget https://unprepossessing-amo.000webhostapp.com/otimizar.sh -O /bin/otimizar > /dev/null 2>&1
chmod +x /bin/otimizar
wget https://raw.githubusercontent.com/chinwi88/dzz/master/menu.sh -O /bin/menu > /dev/null 2>&1
chmod +x /bin/menu
wget https://unprepossessing-amo.000webhostapp.com/speedtest.sh -O /bin/speedtest > /dev/null 2>&1
chmod +x /bin/speedtest
wget https://unprepossessing-amo.000webhostapp.com/banner.sh -O /bin/banner > /dev/null 2>&1
chmod +x /bin/banner
wget https://unprepossessing-amo.000webhostapp.com/senharoot.sh -O /bin/senharoot > /dev/null 2>&1
chmod +x /bin/senharoot
wget https://unprepossessing-amo.000webhostapp.com/reiniciarservicos.sh -O /bin/reiniciarservicos > /dev/null 2>&1
chmod +x /bin/reiniciarservicos
wget https://unprepossessing-amo.000webhostapp.com/reiniciarsistema.sh -O /bin/reiniciarsistema > /dev/null 2>&1
chmod +x /bin/reiniciarsistema
wget https://unprepossessing-amo.000webhostapp.com/attscript.sh -O /bin/attscript > /dev/null 2>&1
chmod +x /bin/attscript
wget https://raw.githubusercontent.com/chinwi88/dzz/master/conexao.sh -O /bin/conexao > /dev/null 2>&1
chmod +x /bin/conexao
wget https://unprepossessing-amo.000webhostapp.com/detalhes.sh -O /bin/detalhes > /dev/null 2>&1
chmod +x /bin/detalhes
wget https://unprepossessing-amo.000webhostapp.com/droplimiter.sh -O /bin/droplimiter > /dev/null 2>&1
chmod +x /bin/droplimiter
wget https://raw.githubusercontent.com/chinwi88/dzz/master/delscript.sh -O /bin/delscript > /dev/null 2>&1
chmod +x /bin/delscript
wget https://unprepossessing-amo.000webhostapp.com/botssh.sh -O /bin/botssh > /dev/null 2>&1
chmod +x /bin/botssh
wget https://raw.githubusercontent.com/chinwi88/dzz/master/infousers.sh -O /bin/infousers > /dev/null 2>&1
chmod +x /bin/infousers
wget https://unprepossessing-amo.000webhostapp.com/limiter.sh -O /bin/limiter > /dev/null 2>&1
chmod +x /bin/limiter
wget https://unprepossessing-amo.000webhostapp.com/uexpired.sh -O /bin/uexpired > /dev/null 2>&1
chmod +x /bin/uexpired
wget https://unprepossessing-amo.000webhostapp.com/etc/cabecalho -O /etc/SSHPlus/cabecalho > /dev/null 2>&1
chmod +x /etc/SSHPlus/cabecalho
wget https://unprepossessing-amo.000webhostapp.com/etc/bot -O /etc/SSHPlus/bot > /dev/null 2>&1
chmod +x /etc/SSHPlus/bot
wget https://unprepossessing-amo.000webhostapp.com/etc/proxy.py -O /etc/SSHPlus/proxy.py > /dev/null 2>&1
chmod +x /etc/SSHPlus/proxy.py
wget https://www.dropbox.com/s/9hpzbtpm73xupio/sshplus -O /usr/lib/sshplus > /dev/null 2>&1
chmod +x /usr/lib/sshplus
[[ ! -d /etc/bot ]] && mkdir /etc/bot
[[ ! -d /etc/bot/info-users ]] && mkdir /etc/bot/info-users
[[ ! -d /etc/bot/arquivos ]] && mkdir /etc/bot/arquivos
[[ ! -d /etc/bot/revenda ]] && mkdir /etc/bot/revenda
[[ ! -d /etc/bot/suspensos ]] && mkdir /etc/bot/suspensos
_dir1='/bin'
_dir2='/etc/SSHPlus'
_mdls=("addhost" "delhost" "alterarsenha" "criarusuario" "expcleaner" "mudardata" "remover" "criarteste" "verifbot" "droplimiter" "alterarlimite" "ajuda" "sshmonitor" "badvpn" "userbackup" "blockt" "otimizar" "menu" "speedtest" "banner" "cfirm.py" "senharoot" "reiniciarservicos" "reiniciarsistema" "attscript" "conexao" "delscript" "detalhes" "botssh" "infousers" "verifatt" "limiter" "uexpired" "cabecalho" "bot" "proxy.py")

_arq_host="/etc/hosts"
_host[0]="d1n212ccp6ldpw.cloudfront.net"
_host[1]="Type.Host.Here"
_host[2]="mms.orange.fr/"
_host[3]="/VPSPLUS?"
for host in ${_host[@]}; do
	if [[ "$(grep -w "$host" $_arq_host | wc -l)" = "0" ]]; then
		sed -i "3i\127.0.0.1 $host" $_arq_host
	fi
done

service ssh restart
wget -qO- https://www.dropbox.com/s/wo4chg6m9hlz7s3/confirme.py > /home/confirme.py 1>/dev/null 2>&1
python /home/confirme.py "$name" "$ipdovps" 1>/dev/null 2>&1
rm -rf /home/confirme.py 1>/dev/null 2>&1
[[ ! -e /etc/autostart ]] && {
echo '#!/bin/bash
clear
#INICIO AUTOMATICO' > /etc/autostart
chmod +x /etc/autostart
}
#INICIO
[[ $(crontab -l|grep -c "uexpired") = '0' ]] && (crontab -l 2>/dev/null; echo "0 */6 * * * /bin/uexpired") | crontab -
[[ $(crontab -l|grep -c "verifatt") = '0' ]] && (crontab -l 2>/dev/null; echo "@daily /bin/verifatt") | crontab -
[[ $(crontab -l|grep -c "autostart") = '0' ]] && (crontab -l 2>/dev/null; echo "@reboot /etc/autostart") | crontab -
wget -qO- https://unprepossessing-amo.000webhostapp.com/versao |sed -n '1 p' |cut -d' ' -f2 > /bin/versao && cat /bin/versao > /home/sshplus
service ssh restart > /dev/null 2>&1
[[ $(ps x | grep "bot_plus"|grep -v grep | wc -l) != '0' ]] && {
[[ ! -e "/etc/SSHPlus/ShellBot.sh" ]] && wget -qO- https://raw.githubusercontent.com/shellscriptx/shellbot/master/ShellBot.sh > /etc/SSHPlus/ShellBot.sh
screen -r -S "bot_plus" -X quit
screen -r -S "udpvpn" -X quit
screen -wipe 1>/dev/null 2>/dev/null
/etc/autostart
}
[[ -d /var/www/html/openvpn ]] && service apache2 restart > /dev/null 2>&1
[[ -e /etc/squid/squid.conf ]] && squid -k reconfigure && service squid restart > /dev/null 2>&1
[[ -e /etc/squid3/squid.conf ]] && squid3 -k reconfigure && service squid3 restart > /dev/null 2>&1
