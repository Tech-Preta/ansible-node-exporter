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
- Antes de tudo, verifique se o Ansible pode se conectar aos hosts. Execute o comando abaixo onde está localizado o seu arquivo de inventário:

```
ansible -i hosts.ini -m ping servidores
```

- Caso tenha sucesso você verá algo como:
```
192.168.0.146 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

6. Crie Seus Playbooks:
- Agora você pode criar seus playbooks Ansible para automatizar tarefas nos servidores.

# Executando o Ansible Node Exporter

Faça o clone do repositório e modifique o inventário de acordo com os servidores que deseja gerenciar. Não se esqueça de seguir os passos anteriores para ter acesso aos servidores.
```
https://github.com/Tech-Preta/ansible-node-exporter.git
```

Para executar o playbook instalar_node_exporter.yml, você pode usar o comando `ansible-playbook`. Certifique-se de estar no diretório onde o playbook está localizado. Aqui está o comando que você pode usar:
```
ansible-playbook instalar_node_exporter.yml --ask-become-pass
```

Se o seu playbook estiver em um diretório específico, você precisará fornecer o caminho completo para o playbook:
```
ansible-playbook /caminho/para/o/playbook/instalar_node_exporter.yml
```

Em caso de sucesso, a saída será semelhante a:

```
PLAY [seus_servidores] *******************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [192.168.0.146]

PLAY [Configurar tipos de chave de host permitidos] **************************************************************************

TASK [Adicionar tipos de chave de host ao cliente SSH] ***********************************************************************
ok: [192.168.0.146] => (item=HostKeyAlgorithms ssh-ed25519,ecdsa-sha2-nistp256,ssh-rsa,rsa-sha2-512,rsa-sha2-256)

PLAY [seus_servidores] *******************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************
ok: [192.168.0.146]

TASK [node_exporter : Baixar o Node Exporter] ********************************************************************************
changed: [192.168.0.146]

TASK [node_exporter : Criar diretório para o Node Exporter] ******************************************************************
ok: [192.168.0.146]

TASK [node_exporter : Extrair o Node Exporter] *******************************************************************************
changed: [192.168.0.146]

TASK [node_exporter : Mover o Node Exporter para /usr/local/bin] *************************************************************
changed: [192.168.0.146]

TASK [node_exporter : Criar usuário para o Node Exporter] ********************************************************************
ok: [192.168.0.146]

TASK [node_exporter : Copiar o serviço systemd do Node Exporter] *************************************************************
changed: [192.168.0.146]

TASK [node_exporter : Definir permissões nos arquivos temporários do Ansible] ************************************************
changed: [192.168.0.146 -> localhost]

TASK [node_exporter : Habilitar e iniciar o serviço do Node Exporter] ********************************************************
ok: [192.168.0.146]

PLAY RECAP *******************************************************************************************************************
192.168.0.146              : ok=11   changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

Certifique-se de ter um arquivo de inventário configurado corretamente e definido os hosts alvo no playbook ou passá-los como argumentos na linha de comando, conforme necessário. Isso garantirá que o Ansible execute as tarefas definidas no playbook nos hosts especificados.
