# Ansible Node Exporter

### Iniciando com o Ansible

1. **Instalação do Ansible**
Certifique-se de que o Ansible está instalado na sua máquina controladora. Caso não esteja, você pode instalá-lo via gerenciador de pacotes (por exemplo, apt, yum, brew, etc.). Se preferir utilize:
```
python3 -m pip install --user ansible
```

2. **Criação de Chaves SSH**
Se você ainda não possui um par de chaves SSH, gere um usando o comando:
```
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
```

- Isso criará as chaves pública (id_rsa.pub) e privada (id_rsa) no diretório ~/.ssh/.

3. **Envio da Chave Pública para o Host de Destino**:

- Copie a chave pública para o host de destino (onde você deseja executar o Ansible). Você pode fazer isso manualmente ou usar o Ansible para automatizar o processo.
- Manualmente:
```
ssh-copy-id -i ~/.ssh/id_rsa.pub user@host
```

- Usando Ansible (em um playbook):
```
---
- hosts: seu_host
  tasks:
    - name: Copiar chave pública para o host
      authorized_key:
        user: seu_usuario
        key: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
```

4. **Configuração do Inventário**
- Crie um arquivo de inventário (por exemplo, hosts.ini) com os detalhes dos hosts que você deseja gerenciar com o Ansible.
- Exemplo de arquivo hosts.ini:
```
[servidores]
servidor1 ansible_host=192.168.1.10
servidor2 ansible_host=192.168.1.20
```

5. **Teste a Conexão SSH**
- Verifique se o Ansible pode se conectar aos hosts:

```
ansible -i hosts.ini -m ping servidores

```
6. Crie Seus Playbooks:
- Agora você pode criar seus playbooks Ansible para automatizar tarefas nos servidores.


