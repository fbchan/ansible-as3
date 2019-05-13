# ansible-as3
Ansible roles to provision application services with AS3 (working in progress)

Example Playbook
---
    - name: Deploy AS3 Apps
      hosts: bigip_mgmt
      gather_facts: false
      connection: local
      roles:
       - tmos-as3-generate-token
       - tmos-as3-apps

       
