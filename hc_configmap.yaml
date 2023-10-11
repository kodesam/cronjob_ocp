apiVersion: v1
data:
  check_status.sh: "#!/bin/sh\n\ncurrent_date=$(date +\"%Y-%m-%d\")\nfilename_prefix=\"status_check_output\"\noutput_log=\"${filename_prefix}_${current_date}.log\"\n\n(\n
    \   #flock is a file locking command to ensure that only one process is updating
    the file\n    flock -x 200\n\n    #get nodes status\n    get_nodes=\"oc get nodes\"\n
    \   echo \"checking node status\" >> \"$output_log\"\n    echo \"#########################################################################################################################\"
    >> \"$output_log\"\n    $get_nodes >> \"$output_log\" 2>&1\n\n    #get clusteroperator
    status\n    get_co=\"oc get co\"\n    echo \"checking clusteroperator status\"
    >> \"$output_log\"\n    echo \"########################################################################################################################\"
    >> \"$output_log\"\n    $get_co >> \"$output_log\" 2>&1\n\n    #get pods status\n
    \   get_pods=\"oc get pods -A | egrep -v 'Running|Completed'\"\n    echo \"checking
    pods status\" >> \"$output_log\"\n    echo \"########################################################################################################################\"
    >> \"$output_log\"\n    eval \"$get_pods\" >> \"$output_log\" 2>&1\n    \n    #get
    top NODE memory and CPU UTIL\n    get_top_node=\"oc adm top nodes\"\n    echo
    \"top memory and cpu util of nodes\"\n    echo \"########################################################################################################################\"
    >> \"$output_log\"\n    eval \"$get_top_node\" >> \"$output_log\" 2>&1\n\n    #
    get top pod memory and cpu UTIL\n    get_top_pod=\"oc adm top pod -A\"\n    echo
    \"top pod cpu and memory util\"\n    echo \"#########################################################################################################################\"
    >> \"$output_log\"\n    eval \"$get_top_pod\" >> \"$output_log\" 2>&1\n\n) 200>\"$output_log.lock\"\n\necho
    \"Commands executed. Outputs and Errors are saved to '$output_log'\"\ncat $output_log\n##using
    shared counterfile to track the cronjob executions\n#increment the counterfile
    value, indicating that the cronjob is completed \nincrement_counter_file(){\n
    \   counter_file=\"counterfile\"\n\n    (\n        #flock is a file locking command
    to ensure only one process is updating the file\n        flock -x 201\n        max_count=8\n
    \       \n        #To check if counterfile is present\n        if [ -e \"$counter_file\"
    ]; then\n            counter=$(cat \"$counter_file\")\n            #check if the
    counter has reached the maximum limit\n            if [ \"$counter\" -eq \"$max_count\"
    ]; then\n                echo \"All cronjobs have completed\"\n\n                #sending
    output to external system\n                ## WIP ##\n\n                #reset
    the counterfile to 0 to keep track of next cronjob execution\n                counter=0\n
    \           else\n                #update the shared counter\n                counter=$((counter+1))
    \n            fi\n        else\n            #if the counterfile doesn't exist,
    create it with an initial value of 1\n            counter=1 \n        fi\n        echo
    \"cronjob completed. counter value: $counter\"\n        echo \"$counter\" > \"$counter_file\"\n
    \   ) 201>\"$counter_file.lock\" \n}\n  \n#move older files(2 days) from current
    directory to tmp directory\ndelete_old_files(){\n  #local directory=\"/home/student/myapp/mygit/testing\"\n
    \ local filename_prefix=\"$1\"\n  find . -type f -name \"${filename_prefix}*\"
    -mtime +2 -exec mv {} /tmp/ \\;\n  echo \"moved the files that are older than
    2 days with prefix '${filename_prefix}'  to /tmp folder from working directory\"\n}\n"
kind: ConfigMap
metadata:
  creationTimestamp: "2023-10-11T14:32:44Z"
  name: myconfigmap-3
  namespace: open-caas-ops
  resourceVersion: "4054266"
  uid: b176f38d-9e94-4771-b9c4-efa48090526d
