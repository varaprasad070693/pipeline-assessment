pipeline {
        agent any
        stages {
                stage ('Build') {
                        steps {
                                sh '''
#!/bin/bash

REPO_LIST="repo_list"
BACKUP_DIR="$WORKSPACE/Backup and Audit"
TODAY=$(date +%Y%m%d)

mkdir -p "$BACKUP_DIR"

while read -r repo_url; do
    repo_name=$(basename "$repo_url" .git)
    repo_path="$BACKUP_DIR/$repo_name"

    if [ ! -d "$repo_path/.git" ]; then
        echo "Cloning started $repo_name..."
        git clone "$repo_url" "$repo_path"
    else
        echo "pulling $repo_name..."
        cd "$repo_path" || continue
        git pull
    fi

    echo "Genearating a tar backup for $repo_name"
    tar -czvf "$BACKUP_DIR/${repo_name}-${today}.tar.gz" -C "$BACKUP_DIR" "$repo_name"

    echo "Searching for recent commits for $repo_name"
    cd "$repo_path" || continue
    git log --since=1.day > "$BACKUP_DIR/audit-${repo_name}-${today}.txt"

done < "$repo_list_file"
'''
                        }
                }
        }
}
