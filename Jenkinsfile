pipeline {
        agent any
        stages {
                stage ('Build') {
                        steps {
                                sh '''
#!/bin/bash
echo "running in /home/ubuntu/script/backup/"
cd /home/ubuntu/script/backup/

name="${/home/ubuntu/script/repo_list}"

                    if [[ -z "$name" ]]; then
                        echo "No filename provided, exiting."
                        exit 1
                    fi

read -p "Pleae enter the filename to start cloning:" name
while read line
do
        git clone $line
        if [ $? -eq 0 ];then
                echo "Clone is successful"
                dir=`echo "$line" | awk -F "." '{print $(NF-1)}'| awk -F "/" '{print $NF}'`
                tar -czf "$dir-$(date +%Y%m%d).tar.gz" $dir
                echo "backup created"
                cd $dir;git log --since=1.day > audit-$(date +%Y%m%d).txt
        else
                sleep 1
                echo "pull reqested"
                #set -x
                dir=`echo "$line" | awk -F "." '{print $(NF-1)}'| awk -F "/" '{print $NF}'`
                if [ -e $dir ];then
                        cd $dir
                        git pull $line
                        git log --since=1.day > audit-$(date +%Y%m%d).txt
                        cd ..
                        #set +x
                        #pwd
                else
                        echo "$line repo is not available"
                fi
        tar -czf "$dir-$(date +%Y%m%d).tar.gz" $dir
        echo "backup created"
        fi
        echo -e "\n"
done < $name
'''
                        }
                }
        }
}
