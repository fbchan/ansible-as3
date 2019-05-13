# ansible-as3
Ansible roles to provision application services with AS3 (working in progress)

Example Playbook
tmos-bigip-as3.yml
---
    - name: Deploy AS3 Apps
      hosts: bigip_mgmt
      gather_facts: false
      connection: local
      roles:
       - tmos-as3-generate-token
       - tmos-as3-apps

Runs from CLI
Deploy AS3 with SSL Offload + AWAF Policy
==========================================
ansible-playbook tmos-bigip-as3.yml -i inventory -e username=admin -e password=admin \
-e bigip_mgmt=10.10.14.94 \
-e app_name=demo2 \
-e app_template=as3_https_appsec \
-e app_vs=1.1.1.11 \
-e awaf_url=https://raw.githubusercontent.com/fbchan/waf-pol/master/awaf-owasp-top10.xml \
-e app_service_port:30002 \
-e '{"app_member_addr":[{"member1":"null","ip_address":"1.1.1.1"},{"member2":"null","ip_address":"2.2.2.2"}]}'

Exaple Output
=============
PLAY [Deploy AS3 Apps] ********************************************************************************************************************************************

TASK [tmos-as3-generate-token : Get Auth token] *******************************************************************************************************************
ok: [bigip_mgmt]

TASK [tmos-as3-generate-token : Display auth_token] ***************************************************************************************************************
ok: [bigip_mgmt] => {
    "msg": "VVKMAF5KWJYAX2S7QU5DHMWTA3"
}

TASK [tmos-as3-generate-token : Copy auth_token to a variable] ****************************************************************************************************
ok: [bigip_mgmt]

TASK [tmos-as3-apps : Build JSON payload] *************************************************************************************************************************
changed: [bigip_mgmt]

TASK [tmos-as3-apps : Deploy AS3 template] ************************************************************************************************************************
ok: [bigip_mgmt]

TASK [tmos-as3-apps : Show apps status when deploy] ***************************************************************************************************************
ok: [bigip_mgmt] => {
    "msg": {
        "code": 200,
        "host": "localhost",
        "lineCount": 31,
        "message": "success",
        "runTime": 8474,
        "tenant": "demo2"
    }
}

TASK [tmos-as3-apps : Delete AS3 template] ************************************************************************************************************************
skipping: [bigip_mgmt]

TASK [tmos-as3-apps : Show apps status when delete] ***************************************************************************************************************
skipping: [bigip_mgmt]

PLAY RECAP ********************************************************************************************************************************************************
bigip_mgmt                 : ok=6    changed=1    unreachable=0    failed=0



