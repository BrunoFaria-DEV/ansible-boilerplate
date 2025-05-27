# Ansible Boilerplate com Vagrant (Debian)

Este projeto é um laboratório de testes utilizando **Ansible** com **Vagrant** e **Debian**. O objetivo é fornecer uma base simples e funcional para automatização de tarefas e configuração de servidores, ideal para estudos ou protótipos.

---

## 📦 Pré-requisitos

- [VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)
- [Vagrant](https://developer.hashicorp.com/vagrant/downloads)
- [Ansible](https://www.ansible.com)

Instale o Ansible no Ubuntu:

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
````

---

## 📁 Estrutura do Projeto

```bash
ansible-boilerplate/
├── ansible.cfg               # Arquivo de configuração local do Ansible
├── hosts                     # Arquivo de inventário local
├── vagrant/
│   ├── Vagrantfile           # Máquina Debian com IP fixo
│   ├── vagrant_key           # Chave privada SSH (gerada manualmente)
│   └── playbooks/
│       ├── file_transfer.yml
│       ├── create_users.yml
│       └── create_users2.yml
```

---

## ⚙️ Configurações

### 🗂 Arquivo de inventário (local)

Utilizamos inventário e configurações **locais** para isolar o ambiente:

* Inventário: `hosts`
* Configuração: `ansible.cfg`

O arquivo `hosts` pode conter:

```ini
[vms]
192.168.56.10 ansible_user=vagrant ansible_ssh_private_key_file=./vagrant/vagrant_key
```

---

## 🚀 Subindo a Máquina com Vagrant

```bash
cd vagrant
vagrant up
vagrant ssh
```

IP fixo: `192.168.56.10`
Box: `debian/bookworm64`
Memória: `1024 MB`

Você pode procurar outras distribuições em:

🔗 [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)

---

## 🔐 Chave SSH

Gere a chave personalizada (caso ainda não tenha):

```bash
ssh-keygen -t rsa -b 4096 -f vagrant/vagrant_key
```

Copie a chave pública para o VM:

```bash
vagrant ssh
# Dentro da VM:
mkdir -p ~/.ssh
cat /vagrant/vagrant_key.pub >> ~/.ssh/authorized_keys
```

---

## ✅ Testando Conexão

```bash
ansible vms -m ping
ansible vms -m shell -a "hostnamectl"
ansible vms -m shell -a "cat /etc/hosts"
```

---

## 📜 Executando Playbooks

```bash
ansible-playbook vagrant/playbooks/file_transfer.yml
ansible-playbook vagrant/playbooks/create_users.yml
ansible-playbook vagrant/playbooks/create_users2.yml
```

Verifique se o usuário foi criado:

```bash
ansible all -m shell -a "getent passwd analistadevops"
ansible all -m shell -a "getent passwd analistadevops2"
```

---

## 📄 Licença

Este projeto está licenciado sob os termos da licença MIT. Veja abaixo:

```
MIT License

Copyright (c) 2025 Bruno

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
