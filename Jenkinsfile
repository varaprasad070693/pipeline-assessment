pipeline {
        agent any
         parameters {
        file(name: 'REPO_FILE', description: 'Upload the repo_list.txt file')
         }
        stages {
                stage ('Build') {
                        steps {
                                sh '''#!/bin/bash
echo "Running in workspace: $WORKSPACE"
mkdir -p "$WORKSPACE/script/backup"
cd "$WORKSPACE/script/backup"

cp "$WORKSPACE/$REPO_FILE" ./repo_list.txt
                    name="repo_list.txt"

                   if [ ! -f "$name" ]; then
                         echo "File $name not found!"
                          exit 1
                  fi

while read -r line;do
echo "Processing $line"
        git clone $line
        if [ $? -eq 0 ];then
                echo "Clone is successful"
                dir=`echo "$line" | awk -F "." '{print $(NF-1)}'| awk -F "/" '{print $NF}'`
                tar -czf "$dir-$(date +%Y%m%d).tar.gz" $dir
                echo "backup created"
                git -c git log --since=1.day > audit-$(date +%Y%m%d).txt
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
