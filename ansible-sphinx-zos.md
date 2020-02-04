# How to generate HTML doc for Ansible collection

# Ensure a working virtual environment
## Clone both repositories, Ansible ans ibm_zos_core
```
git clone git@github.com:ddimatos/ansible.git
```
```
git@github.ibm.com:zos-ansible/ibm_zos_core.git
```

You should have two repositories:
```
ddimatos:[ ~/git/python-venv-ansible-zos ]ls
ansible      ibm_zos_core
```
Change directory into the repository root dir: $ cd ansible
cd ansible

Create a virtual environment: $ python3 -m venv venv

Activate the virtual environment: $ . venv/bin/activate
 
Update the Pip version: pip install --upgrade pip

Install required Python packages:
pip install -r requirements.txt
pip install -r docs/docsite/requirements.txt

These are the required packages, the only one I think not in the requirements and needing installing is:
```pip3 install sphinx-markdown-tables```

pip3 install straight.plugin
pip3 install sphinx 
pip3 install requests 
pip3 install sphinx-markdown-tables
pip3 install sphinx-notfound-page 

```
. hacking/env-setup # not sure you need to really do this unless you are using the runtim

```

mkdir -p lib/ansible/modules/ibm_zos_core/
cp ../ibm_zos_core/plugins/modules/* lib/ansible/modules/ibm_zos_core/
cp ../ibm_zos_core/plugins/connection/zos_ssh.py lib/ansible/plugins/connection/

make clean
MODULES=zos_data_set,zos_job_output,zos_job_query,zos_job_submit  make webdocs
open docs/docsite/_build/html/index.html
open docs/docsite/_build/html/modules/list_of_ibm_zos_core_modules.html
open docs/docsite/_build/html/plugins/connection/zos_ssh.html

cd docs/docsite/_build 
tar -zcvf /tmp/ibm-zos-core-html.tar.gz html/


