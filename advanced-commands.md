Advanced ansible commaands::
----------------------------

1. To check the amount of time a playbook takes to execute:

$ time ansible-playbook site.yml

real	1m38.525s
user	0m14.144s
sys	0m9.955s

2. We don't need to gather facts for each task so we can disable it by:
gather_facts: false


3. Reduce the update_cache=yes by removing it from each playbook's software installation to 
include in the site.yml and set a time of 24 hours:
tasks:
  - name: update apt cache
    apt: update_cache=yes cache_valid_time=86400
    
4. Limit Execution by hosts:
If you want to run the site.yml only on 1 host say `app01` just use the `--limit` option.
$ ansible-playbook site.yml --limit app01

app01 falls under webserver group so it will execute only this and will not consider the database server 
and loadbalancer server.

5. We select what to execute:
$ ansible-playbook site.yml --step 

It will ask for confirmation for each step.

6. Execute from a specific point instead from beginning:
$ ansible-playbook site.yml --list-tasks 
This will list all the task. We can choose the one/ones we want to execute.

ex:
$ ansible-playbook site.yml --start-at-task "copy source code to webapp server"
or
$ ansible-playbook site.yml --limit lb01 --start-at-task "get active sites"


7. Check syntax of yml file:
$ ansible-playbook --syntax-check site.yml 

8. Dry run before actual execution:
$ ansible-playbook --check site.yml

9. Use debug to see what is stored in variables or register:
ex: 
debug: var=vars
or
register: active
debug: var=active.stdout_lines

