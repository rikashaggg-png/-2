        not work (status 200)
        Unknown command verb 'is', did you mean 'isolate'?
        server.sh: line 10: [: missing `]'
        logs
        awk: line 2: missing } near end of file
        [sudo] password for rikkasha: 
        eror
        awk: line 2: missing } near end of file
        tail: cannot open '/vat/log/nginx/access.log' for reading: No such file or directory

мой первый баг после теста , буду исправлять 




        #!/usr/bin bash

        URL="http://localhost:80"
        STATUS=$(curl -o /dev/null -s -w "%{http_code}" "$URL")

                if [[ $STATUS == 200 ]]; then
                echo "not work (status $STATUS)"
                NGINXSTATUS=$(systemctl is active nginx)

                if  [ $NGINXSTATUS = "active"]
                then
                echo "stoped-opening"
                sudo sistemctl start nginx 

                STATUS=$(curl -o /dev/null -s -w "%{http_code}" "$URL")

                echo "open"
                echo "stat code $STATUS"
        
        elif [[ $STATUS == 4** ]]; then
        echo "BRUHHH (error $STATUS)"
        fi

        else 
        echo "WORK  (status $STATUS)"
        fi

        echo "logs"
        sudo tail -n 5 /var/log/nginx/access.log | awk {print "Log;" $0}  

        echo  "eror"
        sudo tail -n 5 /vat/log/nginx/access.log | awk {print "Err;" $0}

^^^^^^^^^^^^^^^^^
моя первая версия










вторая рабочая с исправлением ошибок ( мне помог гпт с их исправлением и нахождением ) >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>












#!/usr/bin bash

URL="http://localhost:80"
STATUS=$(curl -o /dev/null -s -w "%{http_code}" "$URL")

if [[ $STATUS == 200 ]]; then
        echo "not work (status $STATUS)"

        NGINXSTATUS=$(systemctl is-active nginx)

        if  [ "$NGINXSTATUS" = "active" ]
        then
        echo "stoped-opening"
        sudo systemctl start nginx 

        sleep 2

        STATUS=$(curl -o /dev/null -s -w "%{http_code}" "$URL")

        echo "open"
        echo "stat code $STATUS"
        else 
        echo "nginx active $STATUS"
        fi

elif [[ "$STATUS" == 4* ]]; then
        echo "BRUHHH (error $STATUS)"
else 
        echo "WORK  (status $STATUS)"
fi

echo
echo "logs"
sudo tail -n 5 /var/log/nginx/access.log | awk '{print "Log:", $0}'  

echo
echo "eror"
sudo tail -n 5 /var/log/nginx/access.log | awk '{print "Err:", $0}'




