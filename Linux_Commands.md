
#### 1. Utilized space by jenkins in Linux

    df -h /var/lib/jenkins

#### 2. Total size of the folder/directory

    du -sh /var/lib/jenkins/workspace/

#### 3. Size of each directory under the path /var/lib/jenkins/workspace

    du -shc /var/lib/jenkins/workspace/*


#### 4. Print total size, used space, used space percentage, available space

    df -h /var/lib/jenkins

    size=$(df -Ph  /var/lib/jenkins | tail -1 | awk '{print $2}')
    echo ${size}

    used=$(df -Ph  /var/lib/jenkins | tail -1 | awk '{print $3}')
    echo ${used}

    avail=$(df -Ph  /var/lib/jenkins | tail -1 | awk '{print $4}')
    echo ${avail}

    useP=$(df -Ph  /var/lib/jenkins | tail -1 | awk '{print $5}')
    echo ${useP}

#### 5. WAR Files list from the path /var/lib/jenkins/workspace

    loc_to_look='/var/lib/jenkins/workspace'

    file_list=$(find $loc_to_look -type f -name "*.war")

    total_war_size=$(du -ch $file_list | tail -1 | cut -f 1)

    echo '>>>>>>>>>>> Total size of all WAR files is: '$total_war_size

#### 6. Find top 10 largest size directories under the path /var/lib/jenkins/  

    dirs=10

    du -sh /var/lib/jenkins/*  | sort -hr | head -n ${dirs}

#### 7. Find top 10 largest directories under the path /var/lib/jenkins/  and write to text file and print the text file 

    dirs=10

    du -sh /var/lib/jenkins/*  | sort -hr | head -n ${dirs} > topdirs.txt

    cat topdirs.txt

#### 8. find and remove WAR files from the path /var/lib/jenkins/workspace

    loc_to_look='/var/lib/jenkins/workspace'

    command is: find $loc_to_look -type f -name "*.war" -exec rm -f {} +

#### 9. find and remove WAR files from the path /var/lib/jenkins/workspace and write the output to text file for future reference

    loc_to_look='/var/lib/jenkins/workspace'

    echo 'Removing all the WAR files from the path: ' $loc_to_look

    echo "" >> removedPaths.txt

    echo "Removing all the WAR files from the path: " $loc_to_look >> removedPaths.txt

    find $loc_to_look -type f -name "*.war" >> removedPaths.txt

    find $loc_to_look -type f -name "*.war" -exec rm -f {} +


#### 10. Find the list of duplicate jenkins workspace under the path /var/lib/jenkins/workspace


    echo '================Duplicate workspaces created with @2, @3 under /var/lib/jenkins/workspace =============='

    loc_to_look='/var/lib/jenkins/workspace'

    file_list_at2=$(find $loc_to_look -type d -name "*@2")

    total_size_at2=$(du -ch $file_list_at2 | tail -1 | cut -f 1)

    echo '>>>>>>>>>>> Total size of at 2 workspace: ' $total_size_at2

    file_list_at3=$(find $loc_to_look -type d -name "*@3")

    total_size_at3=$(du -ch $file_list_at3 | tail -1 | cut -f 1)

    echo '>>>>>>>>>>> Total size of at 3 workspace: ' $total_size_at3


#### 11. Subdirectories and files details /var/lib/jenkins/workspace

    path=/var/lib/jenkins/workspace

    echo Subdirectories and files details ${path}
    
    ls -R ${path}





