# Install dependencies
```
apt-get update
apt-get install python-pip
```
# Check version
```
ansible --version
```

# SSH Connection
[ANSIBLE - SSH CONNECTION & RUNNING COMMANDS](https://www.bogotobogo.com/DevOps/Ansible/Ansible-SSH-Connection-Setup-Run-Command.php)

## ping EC2
```
ansible all -m ping
```
### Output Sample
> Intance IP | SUCCESS => {
>    "ansible_facts": {
>        "discovered_interpreter_python": "/usr/bin/python"
>    },
>    "changed": false,
>   "ping": "pong"
>}

## whoami: Check User
```
ansible all -a "whoami"
```
## Check OS
```
ansible all -a "uname -a"
```
# Hosts 拆分多個Group進行管理
見 **ansible_hosts** 檔
## 針對特定的Group 下command
### example
只對 `[dev]` 這個Group下command
```
ansible dev -a "python --version"
```
Output
>dev1 | CHANGED | rc=0 >>
>Python 2.7.18
>qa2 | CHANGED | rc=0 >>
>Python 2.7.18

## 自訂組合
### example
只對 `[first]` 這個另外定義的 Group 組合下 command
```
ansible first -a "python --version"
```
Output
>dev1 | CHANGED | rc=0 >>
>Python 2.7.18
>qa1 | CHANGED | rc=0 >>
>Python 2.7.18

## devsubset 管理多個host
當 Hosts 數量一多， 若要快速指定某幾個範圍的host，可用 slice 方式來選擇範圍
只對 `[devsubset]` 指定範圍的 hosts 下 command
### example
`dev[1:2]` 指定 `[dev]` Group 底下的順序為第1-2個host
```
ansible devsubset -a "python --version"
```
Output
>dev2 | CHANGED | rc=0 >>
>Python 2.7.18
>qa1 | CHANGED | rc=0 >>
>Python 2.7.18

## 列出所有的hosts
```
ansible --list-host all
```
output:
>hosts (3):
>    qa1
>    dev1
>    dev2

## 列出指定 Group 包含的 hosts
```
ansible --list-host group
```
### example
```
ansible --list-host first
```
## 排除指定 Group 包含的 hosts
```
ansible --list-host \!first
```
## 依順序列出指定  hosts
```
ansible --list-host qa:dev
```
# Playbook Command
## 執行指定的playbook
```
ansible-playbook playbooks/ping-ec2.yml
```
## 在 playbook 中宣告/使用變數
參考`variables.yml`檔，除了可以在`.yml`檔中宣告的變數給定預設值外，亦可藉由終端機指令重新賦值
### example
賦值 `demoCLI` 給變數`variable1`
```
ansible-playbook playbooks/variables.yml -e variable1=demoCLI
```
## 引入外檔案內含的變數
參考 `vars.yml` 與 `variables.yml` 檔

# 模組化playbooks
參考`modularize_playbooks.yml`檔，使用`import_playbook`語法來匯入其他的 playbooks
## 列出 playbooks 所有的 tasks
```
ansible-playbook playbooks/modularize_playbooks.yml --list-tasks
```
