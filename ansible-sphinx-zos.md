# Generate HTML doc for Ansible collection

## Working directory

Make a working directory for the virtual ennviroment and repositories. Using a virtual environment keeps your system configuration safe and allows for easy tear down.

```
mkdir ~/git/python-venv-ansible-zos
```

## Clone both repositories, __ansible__ and __ibm_zos_core__
```
git clone git@github.com:ddimatos/ansible.git  
git@github.ibm.com:zos-ansible/ibm_zos_core.git
```

You should have two git repositories:
```
$ ls
  ansible      ibm_zos_core
```

Change directory into the repository root dir:
```
$ cd ansible
```

## Ensure a Working Virtual Environment

Create a virtual environment: 
```
$ python3 -m venv venv
```

Activate the virtual environment: 
```
$ . venv/bin/activate
```
 
Update the Pip version in the virtual environment: 
```
pip install --upgrade pip
```

## Prepare virtual environment with `sphinx` dependencies

Install required Python packages:
```
pip install -r requirements.txt
pip install -r docs/docsite/requirements.txt
```

__Note:__
The requirements.txt lack all the pacakges in my latest testing, till I fix that you will want to be sure all these are installed as well, the package usually not installed is __sphinx-markdown-tables__

```
pip3 install straight.plugin
pip3 install sphinx 
pip3 install requests 
pip3 install sphinx-markdown-tables
pip3 install sphinx-notfound-page 

```

## Run the environment setup script for each new dev shell process
This will set up the virtual environment such that you can run ansible-playbook, its quite helpful when you are installing and testing collections and want an easy teardown. I don't believe you need this to generate HTML but for now I have it as a documented step.

```
. hacking/env-setup

```

## Copy the IBM z/OS Core Modules into the build Directory

Make a directory in the build to reflect the IBM Modules:
```
mkdir -p lib/ansible/modules/ibm_zos_core/
```

Copy the IBM z/OS Core Modules and Action Plugins to be Included in the Generated HTML
```
cp ../ibm_zos_core/plugins/modules/* lib/ansible/modules/ibm_zos_core/
cp ../ibm_zos_core/plugins/connection/zos_ssh.py lib/ansible/plugins/connection/
```

# Generate HTML
You can use `clean` after to clean up generated `.rst` and `html` files. Its not necessary to run it the first time.
```
make clean
```

Set the environment varible `IS_ZOS_MODULES` to true if only files that start with prefix `zos_` will be generated, otherwise you should set it to "false";
```
export IS_ZOS_MODULES="true"
```
  
Instruct the Ansible scripts  which modules to build doc for (this will run sphinx):
```
MODULES=zos_data_set,zos_job_output,zos_job_query,zos_job_submit  make webdocs
```

Review the HTML Doc:
```
open docs/docsite/_build/html/index.html
open docs/docsite/_build/html/modules/list_of_ibm_zos_core_modules.html
open docs/docsite/_build/html/plugins/connection/zos_ssh.html
```

Package up the HTML:
```
cd docs/docsite/_build 
tar -zcvf /tmp/ibm-zos-core-html.tar.gz html/
```


