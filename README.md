ABOUT VPS AND raspberry

raspberry auto get ip upto VPS ftp txt

ALSO ， CAN REFLASH TO GET dynamic IP ON raspberry 

BEST THX TEDDY BASH  AND FTP  IDEA

# FTP SERVER
	run at linode web console
	python ftp.py &
		from pyftpdlib.handlers import FTPHandler
		from pyftpdlib.servers import FTPServer
		from pyftpdlib.authorizers import UnixAuthorizer
		from pyftpdlib.filesystems import UnixFilesystem
		def main():
			authorizer = UnixAuthorizer(rejected_users=["root"], require_valid_shell=True)
			handler = FTPHandler
			handler.authorizer = authorizer
			handler.abstracted_fs = UnixFilesystem
			server = FTPServer(('', 21), handler)
			server.serve_forever()
		if __name__ == "__main__":
			main()

	add ur user
		adduser ftp
		passwd ftp
		#enter ur password and enter enter enter

FTP Client
	report my public ip to ftp server with ip.txt
	#1__coding insert DATE&IP to ip.txt
	#2__coding upto ftpserver sh file
	#3__shedule #1 and #2 in /etc/crontab
	
	
	#1__coding insert DATE&IP to ip.txt
		date  > /home/ip.txt  && ifconfig ppp0  >> /home/ip.txt
	#2__coding upto ftpserver sh file
		/home/ftp_upload.sh ip.txt
	#3__shedule #1 and #2 in /etc/corntab
		vim /etc/crontab
			#add schedule
			06 *    * * *   root    date  > /home/ip.txt  && ifconfig ppp0  >> /home/ip.txt
			10 *    * * *   root    cd /home && ./ftp_upload.sh ip.txt

	
	
	
	
	
####################################!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@
ftp_upload.sh
#################!!!!!!!!!!!!!!!!!!@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
#!/usr/bin/env bash
#
# Upload file(s) to FTP server
#
# Copyright (C) 2016 Teddysun <i@teddysun.com>
#
# Argument example:
# 1) ./ftp_upload.sh filename
# 2) ./ftp_upload.sh filename1 filename2 filename3 ...
# 3) ./ftp_upload.sh "*.extension"
# 4) ./ftp_upload.sh "*.extension1" "*.extension2"
#

########## START OF CONFIG ##########

# Local directory (current folder)
LOCALDIR=$( pwd )

# File to log the outcome of backups
LOGFILE="/var/log/ftp_upload.log"

# FTP server
# Enter the Hostname or IP address below
FTP_HOST="192.168.123.456 CHANGE THIS TO YOUR FTP SERVER"

# FTP username
# Enter the FTP username below
FTP_USER="ftp"

# FTP password
# Enter the username's password below
FTP_PASS="YOOOOOOOOO  CHANGE THIS TO YOUR FTP PASSWORD"

# FTP server remote folder
# Enter the FTP remote folder below
# For example: public_html
FTP_DIR="ftp"

########## END OF CONFIG ##########


log() {
    echo "$(date "+%Y-%m-%d %H:%M:%S")" "$1"
    echo -e "$(date "+%Y-%m-%d %H:%M:%S")" "$1" >> ${LOGFILE}
}

# Check ftp command
check_command() {
    if [ ! "$(command -v "ftp")" ]; then
        log "ftp command is not installed, please install it and try again"
        exit 1
    fi
}

# Tranferring backup file to FTP server
ftp_upload() {
    cd ${LOCALDIR} || exit

    [ -z ${FTP_HOST} ] && log "Error: FTP_HOST can not be empty!" && exit 1
    [ -z ${FTP_USER} ] && log "Error: FTP_USER can not be empty!" && exit 1
    [ -z ${FTP_PASS} ] && log "Error: FTP_PASS can not be empty!" && exit 1
    [ -z ${FTP_DIR} ] && log "Error: FTP_DIR can not be empty!" && exit 1

    echo "$@" | grep "*" > /dev/null 2>&1
    if [ $? -eq 0 ]; then
        ls $@ > /dev/null 2>&1
        [ $? -ne 0 ] && log "Error: [$@] file(s) not exists!" && exit 1
    else
        for f in $@
        do
            [ ! -f ${f} ] && log "Error: [${f}] not exists!" && exit 1
        done
    fi

    local FTP_OUT_FILE=("$@")

    log "Tranferring file(s) list below to FTP server:"
    for file in ${FTP_OUT_FILE[@]}
    do
        log "$file"
    done
    ftp -in ${FTP_HOST} 2>&1 >> ${LOGFILE} <<EOF
user $FTP_USER $FTP_PASS
binary
lcd $LOCALDIR
cd $FTP_DIR
mput ${FTP_OUT_FILE[@]}
quit
EOF
    log "Tranfer to FTP server completed"
}


# Main progress
STARTTIME=$(date +%s)

[ $# -eq 0 ] && log "Error: argument can not be empty!" && exit 1

check_command

ftp_upload "$@"


ENDTIME=$(date +%s)
DURATION=$((ENDTIME - STARTTIME))
log "All done"
log "Transfer completed in ${DURATION} seconds"








+++=========================================================================+++
THE IP.TXT WILL REFLASH LOOP IN YOUR FTP SERVER

EX.YOUR RASBERRY IP IS 192.168.55.66

THEN IP.TXT FILE LIKE THIS

root@localhost:/home/ftp# cat ip.txt
三  1月 24 21:10:01 CST 2018
ppp0      Link encap:Point-to-Point Protocol
          inet addr:192.168.55.66
          UP POINTOPOINT RUNNING NOARP MULTICAST  MTU:1492  Metric:1
          RX packets:6669010 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6020218 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:3
          RX bytes:4641068087 (4.6 GB)  TX bytes:4701828837 (4.7 GB)




