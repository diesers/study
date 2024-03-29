Options:
  --ask-vault-pass      
             #ask for vault password
             #加密playbook⽂件时提⽰输⼊密码
  -C, --check           
             #don't make any changes; instead, try to predict some of the changes that may occur
             #模拟执⾏，不会真正在机器上执⾏(查看执⾏会产⽣什么变化)
  -D, --diff            
             #when changing (small) files and templates, show the differences in those files; works great with --check
             #当更新的⽂件数及内容较少时，该选项可显⽰这些⽂件不同的地⽅，该选项结合-C⽤会有较好的效果
  -e EXTRA_VARS, --extra-vars=EXTRA_VARS
             #set additional variables as key=value or YAML/JSON
             #在Playbook中引⼊外部参数变量
  --flush-cache         
             #clear the fact cache
             #将fact清除到的远程主机缓存
  --force-handlers      
             #run handlers even if a task fails
             #强制运⾏handlers的任务，即使在任务失败的情况下
  -f FORKS, --forks=FORKS
             #specify number of parallel processes to use(default=5)
             #并⾏任务数。FORKS被指定为⼀个整数,默认是5
  -h, --help            
             #show this help message and exit
             #打开帮助⽂档API
  -i INVENTORY, --inventory-file=INVENTORY
             #specify inventory host path (default=/etc/ansible/hosts) or comma separated host list.
             #指定要读取的Inventory⽂件
  -l SUBSET, --limit=SUBSET
             #further limit selected hosts to an additional pattern
             #限定执⾏的主机范围
  --list-hosts          
             #outputs a list of matching hosts; does not execute anything else
#列出执⾏匹配到的主机，但并不会执⾏
  --list-tags           
             #list all available tags
             #列出所有可⽤的tags
  --list-tasks          
             #list all tasks that would be executed
             #列出所有即将被执⾏的任务
  -M MODULE_PATH, --module-path=MODULE_PATH
             #specify path(s) to module library (default=None)
             #要执⾏的模块的路径
  --new-vault-password-file=NEW_VAULT_PASSWORD_FILE
             #new vault password file for rekey
             #
  --output=OUTPUT_FILE  
             #output file name for encrypt or decrypt; use - for stdout
             #
  --skip-tags=SKIP_TAGS
             #only run plays and tasks whose tags do not match these values
             #跳过指定的tags任务
  --start-at-task=START_AT_TASK
             #start the playbook at the task matching this name
             #从第⼏条任务(START_AT_TASK)开始执⾏
  --step                
             #one-step-at-a-time: confirm each task before running
             #逐步执⾏Playbook定义的任务，并经⼈⼯确认后继续执⾏下⼀步任务
  --syntax-check        
             #perform a syntax check on the playbook, but do not execute it
#检查Playbook中的语法书写,并不实际执⾏
  -t TAGS, --tags=TAGS  
             #only run plays and tasks tagged with these values
             #指定执⾏该tags的任务
  --vault-password-file=VAULT_PASSWORD_FILE
             #vault password file
             #
  -v, --verbose         
             #verbose mode (-vvv for more, -vvvv to enable connection debugging)
             #执⾏详细输出
  --version             
             #show program's version number and exit
             #显⽰版本
  Connection Options:
    control as whom and how to connect to hosts
    -k, --ask-pass      
             #ask for connection password
             #
    --private-key=PRIVATE_KEY_FILE, --key-file=PRIVATE_KEY_FILE
             #use this file to authenticate the connection
             #
    -u REMOTE_USER, --user=REMOTE_USER
             #connect as this user (default=None)
             #指定远程主机以USERNAME运⾏命令
    -c CONNECTION, --connection=CONNECTION
             #connection type to use (default=smart)
             #指定连接⽅式，可⽤选项paramiko (SSH)、ssh、local，local⽅式常⽤于crontab和kickstarts
    -T TIMEOUT, --timeout=TIMEOUT
             #override the connection timeout in seconds(default=10)
             #SSH连接超时时间设定，默认10s
    --ssh-common-args=SSH_COMMON_ARGS
             #specify common arguments to pass to sftp/scp/ssh (e.g.ProxyCommand
#
    --sftp-extra-args=SFTP_EXTRA_ARGS
             #specify extra arguments to pass to sftp only (e.g. -f, -l)
             #
    --scp-extra-args=SCP_EXTRA_ARGS
             #specify extra arguments to pass to scp only (e.g. -l)
             #
    --ssh-extra-args=SSH_EXTRA_ARGS
             #specify extra arguments to pass to ssh only (e.g. -R)
             #
  Privilege Escalation Options:
    control how and which user you become as on target hosts
    -s, --sudo          
             #run operations with sudo (nopasswd) (deprecated, use become)
             #相当于Linux系统下的sudo命令
    -U SUDO_USER, --sudo-user=SUDO_USER
             #desired sudo user (default=root) (deprecated, use become)
             #使⽤sudo，相当于Linux下的sudo命令
    -S, --su            
             #run operations with su (deprecated, use become)
             #
    -R SU_USER, --su-user=SU_USER
             #run operations with su as this user (default=root)(deprecated, use become)
    -b, --become        
             #run operations with become (does not imply password prompting)
             #
    --become-method=BECOME_METHOD
             #privilege escalation method to use (default=sudo),valid choices: [ sudo | su | pbrun | pfexec | doas |dzdo | ksu | runas ]
             #
--become-user=BECOME_USER
             #run operations as this user (default=root)
             #
    --ask-sudo-pass     
             #ask for sudo password (deprecated, use become)
             #传递sudo密码到远程主机，来保证sudo命令的正常运⾏
    --ask-su-pass       
             #ask for su password (deprecated, use become)
             #
    -K, --ask-become-pass
             #ask for privilege escalation password
             #
