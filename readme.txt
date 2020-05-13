centos-elastic-stack-ansible
Creation d'une infrastructure cento 8 for Elastic Stack (Elasticsearch, Logstash, Kibana, Filebeat & Metricbeat)

-Creer une vm centos 8 et install ansible
     CentOS Linux release 8.1.1911 (Core)
     ansible 2.9.3
     python version = 3.6.8 (default, Nov 21 2019, 19:31:34) [GCC 8.3.1 20190507ed Hat 8.3.1-4)]

-recuperer l'image vdi suivante :
     NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi

-Personnellement l'image etait deposée sur mon windows guest arbo :

     C:\\Users\\n.chastel\\Documents\\Script-deploiement-centos-virtualbox-ansible\\NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi

Si vous choisissez un autre empcament modifier le fichier :VBox_Guests.yml

-cat playbook-elk-cluster/group_vars/VBox_Guests.yml:

VboxInstallPath: 'C:\Program Files\Oracle\VirtualBox'
VboxVMFolderPath: 'C:\Users\n.chastel\VirtualBox VMs'
VdiImageFolder: 'C:\Users\n.chastel\Documents\Script-deploiement-centos-virtualbox-ansible\vbox-provisioning-nat-1centos8'   >>>>>>>>>>>>>>
VdiImageName: 'NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi'    >>>>>>>>>>>>>>>>>>>>>>>>>
AnsiblePublicKey: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC8qfpa1BnlmJtHuxKWindB/F7xSHi/122FRk2J30ehKhnkKs6eTLs5H6oKx8rzFJV9vIY22ZCS1mNvJLrEHBTwA5oQkDPoZVY1C8PGqQf1+53tmeYKDG6Oxc3/SBpYGElOD8RYezr1nGGV7GUJGfjOkNpe2wtkdjl3JavMZThKLnqRTfej3JpDpOOvGGdDtVJrUR07csdXLkswCp4s8VQY8Z7Md+KCHmejbenxmIIY9Y/G1C6SP4YFDhZzvJuMx8iVWR4MJU6MD4A8gxZQfsID3ptyDfUxFv9kavu6Oya04qB6ynK7TkMtmlh+Mh5Vi92mCGle00V9LMa1AUPQ2hVUYis+Xqp/RL0SeX+ZL8AYzerx9bS64zc5Wy92vNAi/U/crXmBjqPn5hx2z+JiAJlxCuwcHA2gbWVkZBSHMhNH8PJC3j4F64K4orjmXoldf9km/QxnX83dJfnhQCZxuWz1Uv9BY+rBZwiEpRlLiEDJsL+E7AxW5oloyu4Q1oDehdk= root@nico-1'


-Command start: ansible-playbook -i ./elastic.hosts ./site.yml

      ansible-playbook -i ./elastic.hosts ./site.yml

-Les variables d'environnements globales ansible sont dans le repertoire 

       playbook-elk-cluster/group_vars/ 

le fichier sites.yml contient l'intrumentation des playbooks 
---
    - import_playbook: deploy_vbox_windows_guest.yml
    - import_playbook: common-centos.yml
    - import_playbook: createROOT-CA.yml
    - import_playbook: elasticsearch.yml
    - import_playbook: kibana.yml
    - import_playbook: logstash.yml
    - import_playbook: filebeat.yml
    - import_playbook: metricbeat.yml
	
Le fichier "elastic.hosts"  décrit l'infrastructure qui va être déployé:

-Fichier elastic.hosts

[VBox_Guests]
elkmaster1 on_hypervisor="192.168.81.1" guest_cpu="1" guest_ram="2048" guest_os_type="RedHat_64" guest_ip="10.0.2.201" guest_gw="10.0.2.1" guest_mask="255.255.255.0" guest_broad="10.0.2.255" guest_dns1="192.168.1.1" guest_domain="gop.link" guest_dns2="80.10.246.2"
elkmaster2 on_hypervisor="192.168.81.1" guest_cpu="1" guest_ram="2048" guest_os_type="RedHat_64" guest_ip="10.0.2.202" guest_gw="10.0.2.1" guest_mask="255.255.255.0" guest_broad="10.0.2.255" guest_dns1="192.168.1.1" guest_domain="gop.link" guest_dns2="80.10.246.2"
elkmaster3 on_hypervisor="192.168.81.1" guest_cpu="1" guest_ram="2048" guest_os_type="RedHat_64" guest_ip="10.0.2.206" guest_gw="10.0.2.1" guest_mask="255.255.255.0" guest_broad="10.0.2.255" guest_dns1="192.168.1.1" guest_domain="gop.link" guest_dns2="80.10.246.2"
kibana1 on_hypervisor="192.168.81.1" guest_cpu="1" guest_ram="2048" guest_os_type="RedHat_64" guest_ip="10.0.2.203" guest_gw="10.0.2.1" guest_mask="255.255.255.0" guest_broad="10.0.2.255" guest_dns1="192.168.1.1" guest_domain="gop.link" guest_dns2="80.10.246.2"
logstash1 on_hypervisor="192.168.81.1" guest_cpu="1" guest_ram="2048" guest_os_type="RedHat_64" guest_ip="10.0.2.204" guest_gw="10.0.2.1" guest_mask="255.255.255.0" guest_broad="10.0.2.255" guest_dns1="192.168.1.1" guest_domain="gop.link" guest_dns2="80.10.246.2"
filebeat1 on_hypervisor="192.168.81.1" guest_cpu="1" guest_ram="2048" guest_os_type="RedHat_64" guest_ip="10.0.2.205" guest_gw="10.0.2.1" guest_mask="255.255.255.0" guest_broad="10.0.2.255" guest_dns1="192.168.1.1" guest_domain="gop.link" guest_dns2="80.10.246.2"

[centos]
10.0.2.201
10.0.2.202
10.0.2.206
10.0.2.203
10.0.2.204
10.0.2.205

[kibana]
10.0.2.203

[logstash]
10.0.2.204

[filebeat]
10.0.2.201
10.0.2.202
10.0.2.206
10.0.2.203
10.0.2.204
10.0.2.205

[metricbeat]
10.0.2.201
10.0.2.202
10.0.2.206
10.0.2.203
10.0.2.204
10.0.2.205

[Provider_VBox_Windows]
192.168.81.1

#group elasticsearch

[elk_cluster:children]
elk_master
elk_nodes

[elk_master]
10.0.2.201
10.0.2.202
10.0.2.206

[elk_nodes]

-Les scripts ansible s'appuient sur des variables globales qui peuvent être modifiés :

playbook-elk-cluster/group_vars:

[root@nico-1 group_vars]# cat all.yml
elasticsearch_version: '7.6.0-1'             >>>>>>>>>>>>>>>>>> Version de la stack a installer            
elasticsearch_cluster_name: nico_cluster     >>>>>>>>>>>>>>>>>> Cluster name
elasticsearch_depot: '7.x'                   >>>>>>>>>>>>>>>>>> Version du depot de la stack ELK
logstash_filebeat_port: '5044'               >>>>>>>>>>>>>>>>>> Port filebeat on logstash server
elk_port: '9200'                             >>>>>>>>>>>>>>>>>> Default port elasticsearch cluster 
kibana_port: '5601'                          >>>>>>>>>>>>>>>>>> Default port kibana interface 
elk_jvm_heap_size: '1g'                      >>>>>>>>>>>>>>>>>> Taille de la JVM elasticsearch
elk_bootstrap_password: 'toto'               >>>>>>>>>>>>>>>>>> Bootstrap password for elasticsearch cluster joining phase (mdp tempo) 
elk_password: 'Euskal1965!'                  >>>>>>>>>>>>>>>>>> Passwd cluster ELK & kibana

-Chaque playbook est idempotent 

Les scripts ansible ont été testés sur image centos 7 & 8 via virtualbox

-Oracle® VM VirtualBox® Version 6.1.4 Edition
-host windows 10 pro i7-8850H CPU 2.60 Ghz 32Go de mémoire.

Soft installés par les playbooks :

-CentOS Linux release 8.1.1911 (Core) 
-Creation certificats auto-signés validité 10ans possibilité de conserver le certificat d'autorité si deja existant.
-installation java-1.8.0-openjdk.x86_64 cf playbook
-elasticsearch_version: '7.6.0-1' (creation de 3 nodes master replica 1 sécurisé SSL/TLS)
-filebeat_version :'7.6.0-1' installation et activation modules system et elasticsearch xpack) 
-logstash_version: '7.6.0-1' (creation pipeline filebeat system. Parse les logs /var/log/secure /var/log/messages)  
-metricbeat_version: '7.6.0-1' (installation et activation system & elk modules )

=================================================================certificats informations====================================================
le fichier contenant les information du certificats auto-signé utilisé :

/etc/ansible/playbook-elk-cluster/createROOT-CA.yml

# This playbook is for create rootCA
# You can run this playbook with the following command:

- hosts: localhost
  remote_user: root
  become: true
  vars:
  - ca_cert: 'rootCA.crt'   
  - ca_key: 'rootCA.key'
  - ca_country: 'FR'                  
  - ca_state: 'France'                >>>state CA 
  - ca_organization: 'Groupe Nico'    >>>entreprise CA
  - ca_commonname: 'PKI Nico'
  roles:
    - role: createRootCA

================================================================Bug constaté sur l'infra deployé en 7.6===============================================

metricbeat apres installation est incapable d'interpreter le dashboard metricbeat system

Dashboard
[Metricbeat System] Host overview ECS sur un intervalle de temps < à 3h. c'est un bug connu

kibana: la timezone appliquée est celle du navigateur ce qui genere des erreurs d'interpretation des dashboard 
Pour contourner le pb après installation on peux setter la timezone depuis kibana

Management/Advanced settings
Timezone for date formatting
Europe/Paris

=================================================================Connexion a kibana===============================================================

kibana url apres redirection de port (virtualbox)

https://127.0.0.1:5601/app/kibana#/home  >>>>creer la redirection de port via l'interface de virutalbox
Compte : elastic
password : Euskal1965! (non crypté dans les playbook pour le moment)

##############################IMPORTANT####################################################

-La strategie ILM n'est pas prise en compte dans les scripts et doir être mise en place manuellement via Kibana

    Kibana/management/IndexManagement
    Kibana/management/IndexLifecyclePolicies
    Kibana/management/RollupJobs

============================================================ en cas de pb a la creation du cluster =========================================================

Tester le cluster en cas de pb a la creation via curl :

curl --cacert /etc/elasticsearch/certs/ca.crt -u elastic:Euskal1965! -ks "https://10.0.2.201:9200/_cluster/health?pretty"

 "cluster_name" : "nico_cluster",
  "status" : "green", >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>etat du cluster
  "timed_out" : false,
  "number_of_nodes" : 3,
  "number_of_data_nodes" : 3,
  "active_primary_shards" : 13,
  "active_shards" : 26,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0

======================================================Deroulement des scripts ansible===================================================

[root@nico-1 playbook-elk-cluster]# ansible-playbook -i ./elastic.hosts ./site.yml

PLAY [VBox_Guests] **************************************************************************************************************************************************************************

TASK [deploy_vbox_windows_guest : Create Local files temporary sub-directory] ***************************************************************************************************************
ok: [elkmaster2 -> localhost]
ok: [logstash1 -> localhost]
ok: [kibana1 -> localhost]
ok: [elkmaster1 -> localhost]
ok: [elkmaster3 -> localhost]
ok: [filebeat1 -> localhost]

TASK [deploy_vbox_windows_guest : Create cloud-init files] **********************************************************************************************************************************
changed: [elkmaster1 -> localhost] => (item={'src': 'user-data.j2', 'dst': 'user-data'})
changed: [logstash1 -> localhost] => (item={'src': 'user-data.j2', 'dst': 'user-data'})
changed: [elkmaster3 -> localhost] => (item={'src': 'user-data.j2', 'dst': 'user-data'})
changed: [elkmaster2 -> localhost] => (item={'src': 'user-data.j2', 'dst': 'user-data'})
changed: [kibana1 -> localhost] => (item={'src': 'user-data.j2', 'dst': 'user-data'})
changed: [logstash1 -> localhost] => (item={'src': 'meta-data.j2', 'dst': 'meta-data'})
changed: [elkmaster1 -> localhost] => (item={'src': 'meta-data.j2', 'dst': 'meta-data'})
changed: [elkmaster3 -> localhost] => (item={'src': 'meta-data.j2', 'dst': 'meta-data'})
changed: [elkmaster2 -> localhost] => (item={'src': 'meta-data.j2', 'dst': 'meta-data'})
changed: [kibana1 -> localhost] => (item={'src': 'meta-data.j2', 'dst': 'meta-data'})
changed: [filebeat1 -> localhost] => (item={'src': 'user-data.j2', 'dst': 'user-data'})
changed: [filebeat1 -> localhost] => (item={'src': 'meta-data.j2', 'dst': 'meta-data'})

TASK [deploy_vbox_windows_guest : Built cloud-init iso] *************************************************************************************************************************************
changed: [logstash1 -> localhost]
changed: [kibana1 -> localhost]
changed: [elkmaster2 -> localhost]
changed: [elkmaster3 -> localhost]
changed: [elkmaster1 -> localhost]
changed: [filebeat1 -> localhost]

TASK [deploy_vbox_windows_guest : Customize Powershell script] ******************************************************************************************************************************
changed: [elkmaster1 -> localhost] => (item={'src': 'VBox-create.j2', 'dst': 'VBox-create-elkmaster1.ps1'})
changed: [elkmaster3 -> localhost] => (item={'src': 'VBox-create.j2', 'dst': 'VBox-create-elkmaster3.ps1'})
changed: [elkmaster2 -> localhost] => (item={'src': 'VBox-create.j2', 'dst': 'VBox-create-elkmaster2.ps1'})
changed: [kibana1 -> localhost] => (item={'src': 'VBox-create.j2', 'dst': 'VBox-create-kibana1.ps1'})
changed: [logstash1 -> localhost] => (item={'src': 'VBox-create.j2', 'dst': 'VBox-create-logstash1.ps1'})
changed: [elkmaster1 -> localhost] => (item={'src': 'VBox-umount-CI.j2', 'dst': 'VBox-umount-CI-elkmaster1.ps1'})
changed: [elkmaster3 -> localhost] => (item={'src': 'VBox-umount-CI.j2', 'dst': 'VBox-umount-CI-elkmaster3.ps1'})
changed: [elkmaster2 -> localhost] => (item={'src': 'VBox-umount-CI.j2', 'dst': 'VBox-umount-CI-elkmaster2.ps1'})
changed: [kibana1 -> localhost] => (item={'src': 'VBox-umount-CI.j2', 'dst': 'VBox-umount-CI-kibana1.ps1'})
changed: [logstash1 -> localhost] => (item={'src': 'VBox-umount-CI.j2', 'dst': 'VBox-umount-CI-logstash1.ps1'})
changed: [filebeat1 -> localhost] => (item={'src': 'VBox-create.j2', 'dst': 'VBox-create-filebeat1.ps1'})
changed: [filebeat1 -> localhost] => (item={'src': 'VBox-umount-CI.j2', 'dst': 'VBox-umount-CI-filebeat1.ps1'})

TASK [deploy_vbox_windows_guest : Copy cloud-init iso to VBox hypervisor host] **************************************************************************************************************
changed: [elkmaster1 -> 192.168.81.1]
changed: [elkmaster2 -> 192.168.81.1]
changed: [logstash1 -> 192.168.81.1]
changed: [elkmaster3 -> 192.168.81.1]
changed: [kibana1 -> 192.168.81.1]
changed: [filebeat1 -> 192.168.81.1]

TASK [deploy_vbox_windows_guest : Copy PowerShell compiled to VBox hypervisor host] *********************************************************************************************************
changed: [elkmaster2 -> 192.168.81.1]
changed: [elkmaster1 -> 192.168.81.1]
changed: [kibana1 -> 192.168.81.1]
changed: [logstash1 -> 192.168.81.1]
changed: [elkmaster3 -> 192.168.81.1]
changed: [filebeat1 -> 192.168.81.1]

TASK [deploy_vbox_windows_guest : run powershell script on VBox hypervisor] *****************************************************************************************************************
changed: [elkmaster2 -> 192.168.81.1]
changed: [logstash1 -> 192.168.81.1]
changed: [elkmaster1 -> 192.168.81.1]
changed: [elkmaster3 -> 192.168.81.1]
changed: [kibana1 -> 192.168.81.1]
changed: [filebeat1 -> 192.168.81.1]

TASK [deploy_vbox_windows_guest : debug] ****************************************************************************************************************************************************
ok: [elkmaster1] => {
    "result.stdout_lines": [
        "Stage1 : Create VM elkmaster1",
        "Stage2 : Create Disk from Cloud Image C:\\Users\\n.chastel\\Documents\\Script-deploiement-centos-virtualbox-ansible\\vbox-provisioning-nat-1centos8\\NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi",
        "Stage3 : Modify VM properties",
        "Stage4 : Create VM Storage device",
        "Stage5 : Starting VM elkmaster1 ...",
        "\telkmaster1 "
    ]
}
ok: [elkmaster2] => {
    "result.stdout_lines": [
        "Stage1 : Create VM elkmaster2",
        "Stage2 : Create Disk from Cloud Image C:\\Users\\n.chastel\\Documents\\Script-deploiement-centos-virtualbox-ansible\\vbox-provisioning-nat-1centos8\\NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi",
        "Stage3 : Modify VM properties",
        "Stage4 : Create VM Storage device",
        "Stage5 : Starting VM elkmaster2 ...",
        "\telkmaster2 "
    ]
}
ok: [elkmaster3] => {
    "result.stdout_lines": [
        "Stage1 : Create VM elkmaster3",
        "Stage2 : Create Disk from Cloud Image C:\\Users\\n.chastel\\Documents\\Script-deploiement-centos-virtualbox-ansible\\vbox-provisioning-nat-1centos8\\NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi",
        "Stage3 : Modify VM properties",
        "Stage4 : Create VM Storage device",
        "Stage5 : Starting VM elkmaster3 ...",
        "\telkmaster3 "
    ]
}
ok: [logstash1] => {
    "result.stdout_lines": [
        "Stage1 : Create VM logstash1",
        "Stage2 : Create Disk from Cloud Image C:\\Users\\n.chastel\\Documents\\Script-deploiement-centos-virtualbox-ansible\\vbox-provisioning-nat-1centos8\\NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi",
        "Stage3 : Modify VM properties",
        "Stage4 : Create VM Storage device",
        "Stage5 : Starting VM logstash1 ...",
        "\tlogstash1 "
    ]
}
ok: [kibana1] => {
    "result.stdout_lines": [
        "Stage1 : Create VM kibana1",
        "Stage2 : Create Disk from Cloud Image C:\\Users\\n.chastel\\Documents\\Script-deploiement-centos-virtualbox-ansible\\vbox-provisioning-nat-1centos8\\NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi",
        "Stage3 : Modify VM properties",
        "Stage4 : Create VM Storage device",
        "Stage5 : Starting VM kibana1 ...",
        "\tkibana1 "
    ]
}
ok: [filebeat1] => {
    "result.stdout_lines": [
        "Stage1 : Create VM filebeat1",
        "Stage2 : Create Disk from Cloud Image C:\\Users\\n.chastel\\Documents\\Script-deploiement-centos-virtualbox-ansible\\vbox-provisioning-nat-1centos8\\NewCentOS-8.1.1911-x86_64-sysprep-lvm.vdi",
        "Stage3 : Modify VM properties",
        "Stage4 : Create VM Storage device",
        "Stage5 : Starting VM filebeat1 ...",
        "\tfilebeat1 "
    ]
}

TASK [deploy_vbox_windows_guest : debug] ****************************************************************************************************************************************************
ok: [elkmaster1] => {
    "result.stderr_lines": [
        "VBoxManage.exe: error: Code VBOX_E_FILE_ERROR (0x80BB0004) - File not accessible or erroneous file contents (extended info not available)",
        "VBoxManage.exe: error: Context: \"CreateMachine(bstrSettingsFile.raw(), bstrName.raw(), ComSafeArrayAsInParam(groups), bstrOsTypeId.raw(), createFlags.raw(), machine.asOutParam())\" at line 280 of file VBoxManageMisc.cpp",
        "VBoxManage.exe: error: Cannot set a new UUID: VERR_VD_IMAGE_READ_ONLY",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(info.asOutParam())\" at line 74 of file VBoxManageControlVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 554 of file VBoxManageModifyVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LaunchVMProcess(a->session, sessionType.raw(), ComSafeArrayAsInParam(aBstrEnv), progress.asOutParam())\" at line 727 of file VBoxManageMisc.cpp",
        "copy-item : Le processus ne peut pas accéder au fichier 'C:\\Users\\n.chastel\\VirtualBox ",
        "VMs\\elkmaster1\\elkmaster1-disk1.vdi', car il est en cours d'utilisation par un autre processus.",
        "Au caractère C:\\temp\\VBox-create-elkmaster1.ps1:39 : 1",
        "+ copy-item $Image $Disk",
        "+ ~~~~~~~~~~~~~~~~~~~~~~",
        "    + CategoryInfo          : NotSpecified: (:) [Copy-Item], IOException",
        "    + FullyQualifiedErrorId : System.IO.IOException,Microsoft.PowerShell.Commands.CopyItemCommand"
    ]
}
ok: [elkmaster2] => {
    "result.stderr_lines": [
        "VBoxManage.exe: error: Code VBOX_E_FILE_ERROR (0x80BB0004) - File not accessible or erroneous file contents (extended info not available)",
        "VBoxManage.exe: error: Context: \"CreateMachine(bstrSettingsFile.raw(), bstrName.raw(), ComSafeArrayAsInParam(groups), bstrOsTypeId.raw(), createFlags.raw(), machine.asOutParam())\" at line 280 of file VBoxManageMisc.cpp",
        "VBoxManage.exe: error: Cannot set a new UUID: VERR_VD_IMAGE_READ_ONLY",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(info.asOutParam())\" at line 74 of file VBoxManageControlVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 554 of file VBoxManageModifyVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LaunchVMProcess(a->session, sessionType.raw(), ComSafeArrayAsInParam(aBstrEnv), progress.asOutParam())\" at line 727 of file VBoxManageMisc.cpp",
        "copy-item : Le processus ne peut pas accéder au fichier 'C:\\Users\\n.chastel\\VirtualBox ",
        "VMs\\elkmaster2\\elkmaster2-disk1.vdi', car il est en cours d'utilisation par un autre processus.",
        "Au caractère C:\\temp\\VBox-create-elkmaster2.ps1:39 : 1",
        "+ copy-item $Image $Disk",
        "+ ~~~~~~~~~~~~~~~~~~~~~~",
        "    + CategoryInfo          : NotSpecified: (:) [Copy-Item], IOException",
        "    + FullyQualifiedErrorId : System.IO.IOException,Microsoft.PowerShell.Commands.CopyItemCommand"
    ]
}
ok: [elkmaster3] => {
    "result.stderr_lines": [
        "VBoxManage.exe: error: Code VBOX_E_FILE_ERROR (0x80BB0004) - File not accessible or erroneous file contents (extended info not available)",
        "VBoxManage.exe: error: Context: \"CreateMachine(bstrSettingsFile.raw(), bstrName.raw(), ComSafeArrayAsInParam(groups), bstrOsTypeId.raw(), createFlags.raw(), machine.asOutParam())\" at line 280 of file VBoxManageMisc.cpp",
        "VBoxManage.exe: error: Cannot set a new UUID: VERR_VD_IMAGE_READ_ONLY",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(info.asOutParam())\" at line 74 of file VBoxManageControlVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 554 of file VBoxManageModifyVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LaunchVMProcess(a->session, sessionType.raw(), ComSafeArrayAsInParam(aBstrEnv), progress.asOutParam())\" at line 727 of file VBoxManageMisc.cpp",
        "copy-item : Le processus ne peut pas accéder au fichier 'C:\\Users\\n.chastel\\VirtualBox ",
        "VMs\\elkmaster3\\elkmaster3-disk1.vdi', car il est en cours d'utilisation par un autre processus.",
        "Au caractère C:\\temp\\VBox-create-elkmaster3.ps1:39 : 1",
        "+ copy-item $Image $Disk",
        "+ ~~~~~~~~~~~~~~~~~~~~~~",
        "    + CategoryInfo          : NotSpecified: (:) [Copy-Item], IOException",
        "    + FullyQualifiedErrorId : System.IO.IOException,Microsoft.PowerShell.Commands.CopyItemCommand"
    ]
}
ok: [logstash1] => {
    "result.stderr_lines": [
        "VBoxManage.exe: error: Code VBOX_E_FILE_ERROR (0x80BB0004) - File not accessible or erroneous file contents (extended info not available)",
        "VBoxManage.exe: error: Context: \"CreateMachine(bstrSettingsFile.raw(), bstrName.raw(), ComSafeArrayAsInParam(groups), bstrOsTypeId.raw(), createFlags.raw(), machine.asOutParam())\" at line 280 of file VBoxManageMisc.cpp",
        "VBoxManage.exe: error: Cannot set a new UUID: VERR_VD_IMAGE_READ_ONLY",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(info.asOutParam())\" at line 74 of file VBoxManageControlVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 554 of file VBoxManageModifyVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LaunchVMProcess(a->session, sessionType.raw(), ComSafeArrayAsInParam(aBstrEnv), progress.asOutParam())\" at line 727 of file VBoxManageMisc.cpp",
        "copy-item : Le processus ne peut pas accéder au fichier 'C:\\Users\\n.chastel\\VirtualBox ",
        "VMs\\logstash1\\logstash1-disk1.vdi', car il est en cours d'utilisation par un autre processus.",
        "Au caractère C:\\temp\\VBox-create-logstash1.ps1:39 : 1",
        "+ copy-item $Image $Disk",
        "+ ~~~~~~~~~~~~~~~~~~~~~~",
        "    + CategoryInfo          : NotSpecified: (:) [Copy-Item], IOException",
        "    + FullyQualifiedErrorId : System.IO.IOException,Microsoft.PowerShell.Commands.CopyItemCommand"
    ]
}
ok: [kibana1] => {
    "result.stderr_lines": [
        "VBoxManage.exe: error: Code VBOX_E_FILE_ERROR (0x80BB0004) - File not accessible or erroneous file contents (extended info not available)",
        "VBoxManage.exe: error: Context: \"CreateMachine(bstrSettingsFile.raw(), bstrName.raw(), ComSafeArrayAsInParam(groups), bstrOsTypeId.raw(), createFlags.raw(), machine.asOutParam())\" at line 280 of file VBoxManageMisc.cpp",
        "VBoxManage.exe: error: Cannot set a new UUID: VERR_VD_IMAGE_READ_ONLY",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(info.asOutParam())\" at line 74 of file VBoxManageControlVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 554 of file VBoxManageModifyVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LaunchVMProcess(a->session, sessionType.raw(), ComSafeArrayAsInParam(aBstrEnv), progress.asOutParam())\" at line 727 of file VBoxManageMisc.cpp",
        "copy-item : Le processus ne peut pas accéder au fichier 'C:\\Users\\n.chastel\\VirtualBox VMs\\kibana1\\kibana1-disk1.vdi', ",
        "car il est en cours d'utilisation par un autre processus.",
        "Au caractère C:\\temp\\VBox-create-kibana1.ps1:39 : 1",
        "+ copy-item $Image $Disk",
        "+ ~~~~~~~~~~~~~~~~~~~~~~",
        "    + CategoryInfo          : NotSpecified: (:) [Copy-Item], IOException",
        "    + FullyQualifiedErrorId : System.IO.IOException,Microsoft.PowerShell.Commands.CopyItemCommand"
    ]
}
ok: [filebeat1] => {
    "result.stderr_lines": [
        "VBoxManage.exe: error: Code VBOX_E_FILE_ERROR (0x80BB0004) - File not accessible or erroneous file contents (extended info not available)",
        "VBoxManage.exe: error: Context: \"CreateMachine(bstrSettingsFile.raw(), bstrName.raw(), ComSafeArrayAsInParam(groups), bstrOsTypeId.raw(), createFlags.raw(), machine.asOutParam())\" at line 280 of file VBoxManageMisc.cpp",
        "VBoxManage.exe: error: Cannot set a new UUID: VERR_VD_IMAGE_READ_ONLY",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(info.asOutParam())\" at line 74 of file VBoxManageControlVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 554 of file VBoxManageModifyVM.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LockMachine(a->session, LockType_Write)\" at line 1048 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"COMGETTER(SystemProperties)(systemProperties.asOutParam())\" at line 331 of file VBoxManageStorageController.cpp",
        "VBoxManage.exe: error: Code REGDB_E_READREGDB (0x80040150) - Could not read key from registry (extended info not available)",
        "VBoxManage.exe: error: Context: \"LaunchVMProcess(a->session, sessionType.raw(), ComSafeArrayAsInParam(aBstrEnv), progress.asOutParam())\" at line 727 of file VBoxManageMisc.cpp",
        "copy-item : Le processus ne peut pas accéder au fichier 'C:\\Users\\n.chastel\\VirtualBox ",
        "VMs\\filebeat1\\filebeat1-disk1.vdi', car il est en cours d'utilisation par un autre processus.",
        "Au caractère C:\\temp\\VBox-create-filebeat1.ps1:39 : 1",
        "+ copy-item $Image $Disk",
        "+ ~~~~~~~~~~~~~~~~~~~~~~",
        "    + CategoryInfo          : NotSpecified: (:) [Copy-Item], IOException",
        "    + FullyQualifiedErrorId : System.IO.IOException,Microsoft.PowerShell.Commands.CopyItemCommand"
    ]
}

TASK [deploy_vbox_windows_guest : Waiting all VM guest are up] ******************************************************************************************************************************
ok: [kibana1 -> localhost] => (item=elkmaster1)
ok: [logstash1 -> localhost] => (item=elkmaster1)
ok: [elkmaster2 -> localhost] => (item=elkmaster1)
ok: [elkmaster3 -> localhost] => (item=elkmaster1)
ok: [elkmaster1 -> localhost] => (item=elkmaster1)
ok: [kibana1 -> localhost] => (item=elkmaster2)
ok: [elkmaster2 -> localhost] => (item=elkmaster2)
ok: [logstash1 -> localhost] => (item=elkmaster2)
ok: [elkmaster1 -> localhost] => (item=elkmaster2)
ok: [elkmaster3 -> localhost] => (item=elkmaster2)
ok: [kibana1 -> localhost] => (item=elkmaster3)
ok: [elkmaster3 -> localhost] => (item=elkmaster3)
ok: [elkmaster1 -> localhost] => (item=elkmaster3)
ok: [elkmaster2 -> localhost] => (item=elkmaster3)
ok: [logstash1 -> localhost] => (item=elkmaster3)
ok: [kibana1 -> localhost] => (item=kibana1)
ok: [elkmaster3 -> localhost] => (item=kibana1)
ok: [elkmaster1 -> localhost] => (item=kibana1)
ok: [logstash1 -> localhost] => (item=kibana1)
ok: [elkmaster2 -> localhost] => (item=kibana1)
ok: [kibana1 -> localhost] => (item=logstash1)
ok: [elkmaster3 -> localhost] => (item=logstash1)
ok: [elkmaster1 -> localhost] => (item=logstash1)
ok: [logstash1 -> localhost] => (item=logstash1)
ok: [elkmaster2 -> localhost] => (item=logstash1)
ok: [kibana1 -> localhost] => (item=filebeat1)
ok: [elkmaster3 -> localhost] => (item=filebeat1)
ok: [elkmaster1 -> localhost] => (item=filebeat1)
ok: [logstash1 -> localhost] => (item=filebeat1)
ok: [elkmaster2 -> localhost] => (item=filebeat1)
ok: [filebeat1 -> localhost] => (item=elkmaster1)
ok: [filebeat1 -> localhost] => (item=elkmaster2)
ok: [filebeat1 -> localhost] => (item=elkmaster3)
ok: [filebeat1 -> localhost] => (item=kibana1)
ok: [filebeat1 -> localhost] => (item=logstash1)
ok: [filebeat1 -> localhost] => (item=filebeat1)

TASK [deploy_vbox_windows_guest : accept new ssh fingerprints] ******************************************************************************************************************************
changed: [elkmaster1 -> localhost] => (item=elkmaster1)
changed: [elkmaster1 -> localhost] => (item=elkmaster2)
changed: [elkmaster1 -> localhost] => (item=elkmaster3)
changed: [elkmaster1 -> localhost] => (item=kibana1)
changed: [elkmaster1 -> localhost] => (item=logstash1)
changed: [elkmaster1 -> localhost] => (item=filebeat1)

TASK [deploy_vbox_windows_guest : Delete remote files] **************************************************************************************************************************************
changed: [elkmaster1 -> 192.168.81.1] => (item=VBox-create-elkmaster1.ps1)
changed: [elkmaster2 -> 192.168.81.1] => (item=VBox-create-elkmaster2.ps1)
changed: [logstash1 -> 192.168.81.1] => (item=VBox-create-logstash1.ps1)
changed: [elkmaster3 -> 192.168.81.1] => (item=VBox-create-elkmaster3.ps1)
changed: [kibana1 -> 192.168.81.1] => (item=VBox-create-kibana1.ps1)
changed: [filebeat1 -> 192.168.81.1] => (item=VBox-create-filebeat1.ps1)

TASK [deploy_vbox_windows_guest : Remove local temporary files] *****************************************************************************************************************************
changed: [elkmaster1 -> localhost] => (item=CI_disk_elkmaster1.iso)
changed: [elkmaster2 -> localhost] => (item=CI_disk_elkmaster2.iso)
changed: [kibana1 -> localhost] => (item=CI_disk_kibana1.iso)
changed: [elkmaster3 -> localhost] => (item=CI_disk_elkmaster3.iso)
changed: [logstash1 -> localhost] => (item=CI_disk_logstash1.iso)
changed: [elkmaster2 -> localhost] => (item=user-data)
changed: [elkmaster1 -> localhost] => (item=user-data)
changed: [kibana1 -> localhost] => (item=user-data)
changed: [elkmaster3 -> localhost] => (item=user-data)
changed: [logstash1 -> localhost] => (item=user-data)
changed: [elkmaster2 -> localhost] => (item=meta-data)
changed: [elkmaster1 -> localhost] => (item=meta-data)
changed: [elkmaster3 -> localhost] => (item=meta-data)
changed: [logstash1 -> localhost] => (item=meta-data)
changed: [kibana1 -> localhost] => (item=meta-data)
changed: [elkmaster2 -> localhost] => (item=VBox-create-elkmaster2.ps1)
changed: [elkmaster1 -> localhost] => (item=VBox-create-elkmaster1.ps1)
changed: [elkmaster3 -> localhost] => (item=VBox-create-elkmaster3.ps1)
changed: [logstash1 -> localhost] => (item=VBox-create-logstash1.ps1)
changed: [kibana1 -> localhost] => (item=VBox-create-kibana1.ps1)
changed: [elkmaster2 -> localhost] => (item=VBox-umount-CI-elkmaster2.ps1)
changed: [elkmaster1 -> localhost] => (item=VBox-umount-CI-elkmaster1.ps1)
changed: [elkmaster3 -> localhost] => (item=VBox-umount-CI-elkmaster3.ps1)
changed: [logstash1 -> localhost] => (item=VBox-umount-CI-logstash1.ps1)
changed: [kibana1 -> localhost] => (item=VBox-umount-CI-kibana1.ps1)
changed: [filebeat1 -> localhost] => (item=CI_disk_filebeat1.iso)
changed: [filebeat1 -> localhost] => (item=user-data)
changed: [filebeat1 -> localhost] => (item=meta-data)
changed: [filebeat1 -> localhost] => (item=VBox-create-filebeat1.ps1)
changed: [filebeat1 -> localhost] => (item=VBox-umount-CI-filebeat1.ps1)

PLAY [centos] *******************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************
ok: [10.0.2.204]
ok: [10.0.2.203]
ok: [10.0.2.206]
ok: [10.0.2.202]
ok: [10.0.2.201]
ok: [10.0.2.205]

TASK [basics : set_fact] ********************************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.206]
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [basics : Install basic packages] ******************************************************************************************************************************************************
ok: [10.0.2.203]
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.204]
ok: [10.0.2.206]
ok: [10.0.2.205]

TASK [basics : Set up firewall] *************************************************************************************************************************************************************
changed: [10.0.2.206]
changed: [10.0.2.201]
changed: [10.0.2.203]
changed: [10.0.2.202]
changed: [10.0.2.204]
changed: [10.0.2.205]

TASK [basics : Start the firewall] **********************************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.203]
changed: [10.0.2.204]
changed: [10.0.2.205]

TASK [basics : Install epel from remote repo] ***********************************************************************************************************************************************
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.206]
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.205]

TASK [basics : Install basic Epel packages] *************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.206]
ok: [10.0.2.203]
ok: [10.0.2.202]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [timesync : Determine current NTP provider] ********************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.203]
ok: [10.0.2.206]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [timesync : Select NTP provider] *******************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.206]
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [timesync : Install chrony] ************************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.203]
ok: [10.0.2.206]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [timesync : Install ntp] ***************************************************************************************************************************************************************
skipping: [10.0.2.201]
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]

TASK [timesync : Get chrony version] ********************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.203]
ok: [10.0.2.206]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [timesync : Get ntp version] ***********************************************************************************************************************************************************
skipping: [10.0.2.201]
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]

TASK [timesync : Generate chrony.conf file] *************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.206]
ok: [10.0.2.202]
ok: [10.0.2.204]
ok: [10.0.2.203]
ok: [10.0.2.205]

TASK [timesync : Generate chronyd sysconfig file] *******************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.206]
ok: [10.0.2.204]
ok: [10.0.2.203]
ok: [10.0.2.202]
ok: [10.0.2.205]

TASK [timesync : Generate ntp.conf file] ****************************************************************************************************************************************************
skipping: [10.0.2.201]
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]

TASK [timesync : Generate ntpd sysconfig file] **********************************************************************************************************************************************
skipping: [10.0.2.201]
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]

TASK [timesync : Update network sysconfig file] *********************************************************************************************************************************************
ok: [10.0.2.206]
ok: [10.0.2.201]
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.202]
ok: [10.0.2.205]

TASK [timesync : Disable chronyd] ***********************************************************************************************************************************************************
skipping: [10.0.2.201]
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]

TASK [timesync : Disable ntpd] **************************************************************************************************************************************************************
fatal: [10.0.2.206]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpd: host"}
...ignoring
fatal: [10.0.2.203]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpd: host"}
...ignoring
fatal: [10.0.2.201]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpd: host"}
...ignoring
fatal: [10.0.2.204]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpd: host"}
...ignoring
fatal: [10.0.2.202]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpd: host"}
...ignoring
fatal: [10.0.2.205]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpd: host"}
...ignoring

TASK [timesync : Disable ntpdate] ***********************************************************************************************************************************************************
fatal: [10.0.2.201]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpdate: host"}
...ignoring
fatal: [10.0.2.206]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpdate: host"}
...ignoring
fatal: [10.0.2.202]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpdate: host"}
...ignoring
fatal: [10.0.2.204]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpdate: host"}
...ignoring
fatal: [10.0.2.203]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpdate: host"}
...ignoring
fatal: [10.0.2.205]: FAILED! => {"changed": false, "msg": "Could not find the requested service ntpdate: host"}
...ignoring

TASK [timesync : Enable chronyd] ************************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.203]
ok: [10.0.2.206]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [timesync : Enable ntpd] ***************************************************************************************************************************************************************
skipping: [10.0.2.201]
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]

TASK [timesync : Set timezone] **************************************************************************************************************************************************************
ok: [10.0.2.206]
ok: [10.0.2.203]
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [updateetchosts : Update the /etc/hosts file with node name] ***************************************************************************************************************************
ok: [10.0.2.201] => (item=10.0.2.201)
ok: [10.0.2.206] => (item=10.0.2.201)
ok: [10.0.2.202] => (item=10.0.2.201)
ok: [10.0.2.203] => (item=10.0.2.201)
ok: [10.0.2.204] => (item=10.0.2.201)
ok: [10.0.2.201] => (item=10.0.2.202)
ok: [10.0.2.204] => (item=10.0.2.202)
ok: [10.0.2.203] => (item=10.0.2.202)
ok: [10.0.2.206] => (item=10.0.2.202)
ok: [10.0.2.202] => (item=10.0.2.202)
ok: [10.0.2.201] => (item=10.0.2.206)
ok: [10.0.2.206] => (item=10.0.2.206)
ok: [10.0.2.202] => (item=10.0.2.206)
ok: [10.0.2.204] => (item=10.0.2.206)
ok: [10.0.2.203] => (item=10.0.2.206)
ok: [10.0.2.201] => (item=10.0.2.203)
ok: [10.0.2.203] => (item=10.0.2.203)
ok: [10.0.2.202] => (item=10.0.2.203)
ok: [10.0.2.204] => (item=10.0.2.203)
ok: [10.0.2.206] => (item=10.0.2.203)
ok: [10.0.2.201] => (item=10.0.2.204)
ok: [10.0.2.203] => (item=10.0.2.204)
ok: [10.0.2.202] => (item=10.0.2.204)
ok: [10.0.2.206] => (item=10.0.2.204)
ok: [10.0.2.204] => (item=10.0.2.204)
ok: [10.0.2.201] => (item=10.0.2.205)
ok: [10.0.2.203] => (item=10.0.2.205)
ok: [10.0.2.202] => (item=10.0.2.205)
ok: [10.0.2.204] => (item=10.0.2.205)
ok: [10.0.2.206] => (item=10.0.2.205)
ok: [10.0.2.205] => (item=10.0.2.201)
ok: [10.0.2.205] => (item=10.0.2.202)
ok: [10.0.2.205] => (item=10.0.2.206)
ok: [10.0.2.205] => (item=10.0.2.203)
ok: [10.0.2.205] => (item=10.0.2.204)
ok: [10.0.2.205] => (item=10.0.2.205)

PLAY [localhost] ****************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************
ok: [localhost]

TASK [createRootCA : Check if /etc/pki/CA/private/rootCA.key certificate exsists] ***********************************************************************************************************
ok: [localhost]

TASK [createRootCA : verify /etc/pki/CA/private/rootCA.key status] **************************************************************************************************************************
ok: [localhost] => {
    "msg": "/etc/pki/CA/private/rootCA.key is present - setting fact _ca_key_exists [ internal fact with _ prefix ] to true - this role will not generate a new one"
}

TASK [createRootCA : set _ca_key_exists var to true] ****************************************************************************************************************************************
ok: [localhost]

TASK [createRootCA : Show _ca_key_exists value] *********************************************************************************************************************************************
ok: [localhost] => {
    "msg": "variable _ca_key_exists is set to False"
}

TASK [createRootCA : Check if /etc/pki/CA/certs/rootCA.crt certificate exsists] *************************************************************************************************************
ok: [localhost]

TASK [createRootCA : Check certificate expiration date [ Expecting more than a week before setting regeneration var ... ]] ******************************************************************
changed: [localhost]

TASK [createRootCA : debug] *****************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "/etc/pki/CA/certs/rootCA.crt is still valid no need to generate"
}

TASK [createRootCA : set_fact] **************************************************************************************************************************************************************
ok: [localhost]

TASK [createRootCA : Show variable value] ***************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "\" _ca_key_exists: False\"\n\"_ca_missing_cert: False\"\n\"ca_force_create: False\"\n"
}

TASK [createRootCA : include_tasks] *********************************************************************************************************************************************************
skipping: [localhost]

PLAY [elk_cluster] **************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.206]
ok: [10.0.2.202]

TASK [java : Install Oracle openJDK 8] ******************************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]

TASK [elastic_stack : Create elasticsearch 7.x repository] **********************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.201]
changed: [10.0.2.206]

TASK [elasticsearch : Install elasticsearch] ************************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.206]
changed: [10.0.2.202]

TASK [elasticsearch : Create elasticsearch config] ******************************************************************************************************************************************
changed: [10.0.2.206]
changed: [10.0.2.201]
changed: [10.0.2.202]

TASK [elasticsearch : copie jvm option template] ********************************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.206]

TASK [elasticsearch : create elasticsearch.service.d directory] *****************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.201]

TASK [elasticsearch : copie elastic service settings] ***************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]

TASK [elasticsearch : Force systemd to reread configs] **************************************************************************************************************************************
ok: [10.0.2.202]
ok: [10.0.2.201]
ok: [10.0.2.206]

TASK [elasticsearch : Enable elasticsearch.service] *****************************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.201]
changed: [10.0.2.206]

TASK [elasticsearch : permit port 9200 for elasticsearch REST] ******************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]

TASK [elasticsearch : permit port 9300 for elasticsearch node communication] ****************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.206]
changed: [10.0.2.202]

TASK [elasticsearch : restart firewalld service] ********************************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.201]
changed: [10.0.2.206]

TASK [elasticsearch : Check if /etc/elasticsearch/certs/elkmaster1.key already exists] ******************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.206]

TASK [elasticsearch : Create certificate directory] *****************************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.201]
changed: [10.0.2.206]

TASK [elasticsearch : Copy the CA certificate and CA provate key  from ansible to remote node] **********************************************************************************************
changed: [10.0.2.202] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.201] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.206] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.202] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.201] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.206] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})

TASK [elasticsearch : Generate the client private key file] *********************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]

TASK [elasticsearch : Generate the CSR file signed with the private key] ********************************************************************************************************************
changed: [10.0.2.206]
changed: [10.0.2.202]
changed: [10.0.2.201]

TASK [elasticsearch : Sign the CSR file with my ower authorithy] ****************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.201]
changed: [10.0.2.206]

TASK [elasticsearch : check for bootstrap password] *****************************************************************************************************************************************
ok: [10.0.2.202]
ok: [10.0.2.201]
ok: [10.0.2.206]

TASK [elasticsearch : set bootstrap password] ***********************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]

TASK [elasticsearch : Start elasticsearch.service] ******************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.206]
changed: [10.0.2.202]

TASK [elasticsearch : Pause play ( Max 2 minutes ) until ELK Cluster status is Green] *******************************************************************************************************
skipping: [10.0.2.202]
skipping: [10.0.2.206]
ok: [10.0.2.201]

TASK [elasticsearch : Change into elk security base the elastic password] *******************************************************************************************************************
skipping: [10.0.2.202]
skipping: [10.0.2.206]
ok: [10.0.2.201]

TASK [elasticsearch : Pause play ( Max 2 minutes ) until ELK Cluster status is Green] *******************************************************************************************************
skipping: [10.0.2.202]
skipping: [10.0.2.206]
ok: [10.0.2.201]

TASK [elasticsearch : Activate Xpack Monitoring Collections in elk] *************************************************************************************************************************
skipping: [10.0.2.202]
skipping: [10.0.2.206]
ok: [10.0.2.201]

PLAY [kibana] *******************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************
ok: [10.0.2.203]

TASK [java : Install Oracle openJDK 8] ******************************************************************************************************************************************************
changed: [10.0.2.203]

TASK [elastic_stack : Create elasticsearch 7.x repository] **********************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Install kibana] **************************************************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Create kibana config] ********************************************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Force systemd to reread configs] *********************************************************************************************************************************************
ok: [10.0.2.203]

TASK [kibana : Enable kibana.service] *******************************************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : permit port 5601 for kibana] *************************************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : restart firewalld service] ***************************************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Check if /etc/elasticsearch/certs/kibana1.key already exists] ****************************************************************************************************************
ok: [10.0.2.203]

TASK [kibana : Create certificate directory] ************************************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Copy the CA certificate and CA provate key  from ansible to remote node] *****************************************************************************************************
changed: [10.0.2.203] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.203] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})

TASK [kibana : Generate the client private key file] ****************************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Generate the CSR file signed with the private key] ***************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Sign the CSR file with my ower authorithy] ***********************************************************************************************************************************
changed: [10.0.2.203]

TASK [kibana : Start kibana.service] ********************************************************************************************************************************************************
changed: [10.0.2.203]

PLAY [logstash] *****************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************
ok: [10.0.2.204]

TASK [java : Install Oracle openJDK 8] ******************************************************************************************************************************************************
changed: [10.0.2.204]

TASK [elastic_stack : Create elasticsearch 7.x repository] **********************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Install logstash] **********************************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Create logstash config] ****************************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Create logstash pipeline for filebeat] *************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Force systemd to reread configs] *******************************************************************************************************************************************
ok: [10.0.2.204]

TASK [logstash : Enable logstash.service] ***************************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : permit port 5044 for logstash] *********************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : restart firewalld service] *************************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Check if /etc/logstash/certs/logstash1.key already exists] *****************************************************************************************************************
ok: [10.0.2.204]

TASK [logstash : Create certificate directory] **********************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Copy the CA certificate and CA provate key  from ansible to remote node] ***************************************************************************************************
changed: [10.0.2.204] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.204] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})

TASK [logstash : Generate the client private key file] **************************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Generate the CSR file signed with the private key] *************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Sign the CSR file with my ower authorithy] *********************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Convert filebeat key to pks8 extension for input module logstash lumberjack protocol] **************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Change file permission on key file pks8] ***********************************************************************************************************************************
changed: [10.0.2.204]

TASK [logstash : Pause play ( Max 2 minutes ) until Kibana status is Green] *****************************************************************************************************************
ok: [10.0.2.204]

TASK [logstash : Start logstash.service] ****************************************************************************************************************************************************
changed: [10.0.2.204]

PLAY [filebeat] *****************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************
ok: [10.0.2.204]
ok: [10.0.2.203]
ok: [10.0.2.201]
ok: [10.0.2.206]
ok: [10.0.2.202]
ok: [10.0.2.205]

TASK [elastic_stack : Create elasticsearch 7.x repository] **********************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.206]
ok: [10.0.2.203]
ok: [10.0.2.204]
changed: [10.0.2.205]

TASK [filebeat : Install filebeat] **********************************************************************************************************************************************************
changed: [10.0.2.204]
changed: [10.0.2.205]
changed: [10.0.2.203]
changed: [10.0.2.201]
changed: [10.0.2.206]
changed: [10.0.2.202]

TASK [filebeat : Create filebeat config] ****************************************************************************************************************************************************
changed: [10.0.2.203]
changed: [10.0.2.201]
changed: [10.0.2.206]
changed: [10.0.2.202]
changed: [10.0.2.204]
changed: [10.0.2.205]

TASK [filebeat : Copy system modules config] ************************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.203]
changed: [10.0.2.202]
changed: [10.0.2.204]
changed: [10.0.2.206]
changed: [10.0.2.205]

TASK [filebeat : Copy filebeat elasticsearch modules on elk cluster node only] **************************************************************************************************************
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]
changed: [10.0.2.206]
changed: [10.0.2.201]
changed: [10.0.2.202]

TASK [filebeat : Force systemd to reread configs] *******************************************************************************************************************************************
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.202]
ok: [10.0.2.206]
ok: [10.0.2.201]
ok: [10.0.2.205]

TASK [filebeat : Enable filebeat.service] ***************************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.204]
changed: [10.0.2.206]
changed: [10.0.2.202]
changed: [10.0.2.203]
changed: [10.0.2.205]

TASK [filebeat : Check if /etc/filebeat/certs/elkmaster1.key already exists] ****************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.206]
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.205]

TASK [filebeat : Create certificate directory] **********************************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.201]
changed: [10.0.2.203]
changed: [10.0.2.204]
changed: [10.0.2.205]

TASK [filebeat : Copy the CA certificate and CA provate key  from ansible to remote node] ***************************************************************************************************
changed: [10.0.2.201] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.202] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.206] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.203] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.204] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.204] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.202] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.201] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.206] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.203] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.205] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.205] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})

TASK [filebeat : Generate the client private key file] **************************************************************************************************************************************
changed: [10.0.2.204]
changed: [10.0.2.201]
changed: [10.0.2.206]
changed: [10.0.2.202]
changed: [10.0.2.205]
changed: [10.0.2.203]

TASK [filebeat : Generate the CSR file signed with the private key] *************************************************************************************************************************
changed: [10.0.2.206]
changed: [10.0.2.202]
changed: [10.0.2.201]
changed: [10.0.2.204]
changed: [10.0.2.203]
changed: [10.0.2.205]

TASK [filebeat : Sign the CSR file with my ower authorithy] *********************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.204]
changed: [10.0.2.203]
changed: [10.0.2.205]

TASK [filebeat : Convert filebeat key to pks8 extension for input module logstash lumberjack protocol] **************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.204]
changed: [10.0.2.203]
changed: [10.0.2.205]

TASK [filebeat : Change file permission on key file pks8] ***********************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.204]
changed: [10.0.2.203]
changed: [10.0.2.206]
changed: [10.0.2.205]

TASK [filebeat : Pause play ( Max 2 minutes ) until Kibana status is Green] *****************************************************************************************************************
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.202]
ok: [10.0.2.206]
ok: [10.0.2.201]
ok: [10.0.2.205]

TASK [filebeat : setup pipeline module system] **********************************************************************************************************************************************
changed: [10.0.2.203]
changed: [10.0.2.204]
changed: [10.0.2.205]
changed: [10.0.2.206]
changed: [10.0.2.202]
changed: [10.0.2.201]

TASK [filebeat : setup pipeline module elasticsearch for elk cluster node] ******************************************************************************************************************
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]
changed: [10.0.2.201]

TASK [filebeat : setup dashboard load to kibana] ********************************************************************************************************************************************
skipping: [10.0.2.202]
skipping: [10.0.2.206]
skipping: [10.0.2.203]
skipping: [10.0.2.204]
skipping: [10.0.2.205]
changed: [10.0.2.201]

TASK [filebeat : Start filebeat.service] ****************************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.204]
changed: [10.0.2.202]
changed: [10.0.2.203]
changed: [10.0.2.206]
changed: [10.0.2.205]

PLAY [metricbeat] ***************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************
ok: [10.0.2.204]
ok: [10.0.2.201]
ok: [10.0.2.203]
ok: [10.0.2.205]
ok: [10.0.2.202]
ok: [10.0.2.206]

TASK [elastic_stack : Create elasticsearch 7.x repository] **********************************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.202]
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.206]
ok: [10.0.2.205]

TASK [metricbeat : Install metribeat] *******************************************************************************************************************************************************
changed: [10.0.2.204]
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.205]
changed: [10.0.2.203]

TASK [metricbeat : Create metricbeat config] ************************************************************************************************************************************************
changed: [10.0.2.203]
changed: [10.0.2.204]
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.205]

TASK [metricbeat : Deploy system module config] *********************************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.201]
changed: [10.0.2.204]
changed: [10.0.2.203]
changed: [10.0.2.206]
changed: [10.0.2.205]

TASK [metricbeat : Deploy elasticsearch xpack module config] ********************************************************************************************************************************
changed: [10.0.2.203]
changed: [10.0.2.201]
changed: [10.0.2.204]
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.205]

TASK [metricbeat : enable module xpack] *****************************************************************************************************************************************************
changed: [10.0.2.203]
changed: [10.0.2.204]
changed: [10.0.2.205]
changed: [10.0.2.201]
changed: [10.0.2.206]
changed: [10.0.2.202]

TASK [metricbeat : Check if /etc/metricbeat/certs/elkmaster1.key already exists] ************************************************************************************************************
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.201]
ok: [10.0.2.206]
ok: [10.0.2.202]
ok: [10.0.2.205]

TASK [metricbeat : Create certificate directory] ********************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.204]
changed: [10.0.2.203]
changed: [10.0.2.206]
changed: [10.0.2.205]

TASK [metricbeat : Copy the CA certificate and CA provate key  from ansible to remote node] *************************************************************************************************
changed: [10.0.2.203] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.206] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.201] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.202] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.204] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.204] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.203] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.206] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.202] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.201] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})
changed: [10.0.2.205] => (item={'src': 'private/rootCA.key', 'dst': 'rootCA.key'})
changed: [10.0.2.205] => (item={'src': 'certs/rootCA.crt', 'dst': 'rootCA.crt'})

TASK [metricbeat : Generate the client private key file] ************************************************************************************************************************************
changed: [10.0.2.203]
changed: [10.0.2.204]
changed: [10.0.2.206]
changed: [10.0.2.202]
changed: [10.0.2.205]
changed: [10.0.2.201]

TASK [metricbeat : Generate the CSR file signed with the private key] ***********************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.203]
changed: [10.0.2.206]
changed: [10.0.2.204]
changed: [10.0.2.205]

TASK [metricbeat : Sign the CSR file with my ower authorithy] *******************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.202]
changed: [10.0.2.203]
changed: [10.0.2.206]
changed: [10.0.2.204]
changed: [10.0.2.205]

TASK [metricbeat : Pause play ( Max 2 minutes ) until Kibana status is Green] ***************************************************************************************************************
ok: [10.0.2.201]
ok: [10.0.2.203]
ok: [10.0.2.204]
ok: [10.0.2.206]
ok: [10.0.2.202]
ok: [10.0.2.205]

TASK [metricbeat : Load Dashboard and indexes] **********************************************************************************************************************************************
changed: [10.0.2.201]

TASK [metricbeat : Enable metricbeat.service] ***********************************************************************************************************************************************
changed: [10.0.2.201]
changed: [10.0.2.204]
changed: [10.0.2.203]
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.205]

TASK [metricbeat : Start metricbeat.service] ************************************************************************************************************************************************
changed: [10.0.2.202]
changed: [10.0.2.206]
changed: [10.0.2.201]
changed: [10.0.2.203]
changed: [10.0.2.204]
changed: [10.0.2.205]

PLAY RECAP **********************************************************************************************************************************************************************************
10.0.2.201                 : ok=83   changed=48   unreachable=0    failed=0    skipped=6    rescued=0    ignored=2
10.0.2.202                 : ok=76   changed=45   unreachable=0    failed=0    skipped=12   rescued=0    ignored=2
10.0.2.203                 : ok=69   changed=40   unreachable=0    failed=0    skipped=9    rescued=0    ignored=2
10.0.2.204                 : ok=73   changed=43   unreachable=0    failed=0    skipped=9    rescued=0    ignored=2
10.0.2.205                 : ok=53   changed=28   unreachable=0    failed=0    skipped=9    rescued=0    ignored=2
10.0.2.206                 : ok=76   changed=45   unreachable=0    failed=0    skipped=12   rescued=0    ignored=2
elkmaster1                 : ok=13   changed=9    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
elkmaster2                 : ok=12   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
elkmaster3                 : ok=12   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
filebeat1                  : ok=12   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
kibana1                    : ok=12   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=10   changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
logstash1                  : ok=12   changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0




