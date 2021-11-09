# Deploy-MDATP-for-Linux-using-Ansible
Following ansible playbooks help address the deployment and configuration of MDATP for Linux agents using Ansible.

**NOTE:** 
1. You need to have a basic understanding of how Ansible works
2. These playbooks are intended for Linux workloads
3. Tested with RHEL 7 (please do a test run for any other flavours of Linux)
4. Refer Microsoft MDATP documentation for pre-requisites: [Defender for Linux](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux?view=o365-worldwide)

The following playbooks are for deploying and configuring MDATP for Linux in a enterprise environment. These are tested in a production environment and significantly reduced the time to deploy the MDATP agents, on board and configure them.

The playbooks are indented to do the following tasks:
1. Uninstall any existing Anti-Malware solution (In my case, it was Symantec Endpoint Protection)
2. Configure the Microsoft Production Repository
3. Install MDATP agent on the Linux workloads
4. On-board the MDATP agents to Defender ATP / Defender 365
5. Push the configuration file for MDATP agents
6. Configure the scheduled scan using the CRON module


## List of Playbooks:
1. **Install_MDATP.yaml**
2. **Configure_MDATP.yaml**
3. **Config_exclusion.yaml**
4. **Uninstall_MDATP.yaml**

## List of Roles:
1. **Uninstall_SEP.yaml**
2. **Add_YUM_Repo.yaml**
3. **Onboarding_setup.yaml**
4. **Config_MDATP.yam**

# Instructions

## Pre-requsites
1. Make sure your ansible server has ssh connectivty to the nodes you want to deploy
2. Make sure the required files are placed in **/etc/ansible/files**

## How to use the playbooks?
### For a new deployment
1. Use the **Install_MDATP.yaml** playbook to:
   - **(Optional, if you have an existing AV)** Uninstalls existing AV (In my Case it is Symantec)
   - Executes the on boarding tasks
   - Configures the Microsoft production repository
   - Installs the MDATP agent
2. Now execute **Configure_MDATP.yaml** playbook to:
   - Push the MDATP configuration file **mdatp_managed.json**
   - Restarts the MDATP service for the configuration to reflect
   - Configures Daily quick scan and Weekly full scan using the ansible cron module

### For rolling out exclusions
1. Use **Config_exclusion.yaml** playbook to roll out the exclusions for MDATP for Linux
2. Prior to rolling out the **mdatp_managed.json** make sure it is configured as per [Microsoft's Recommendations](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/linux-preferences?view=o365-worldwide#full-configuration-profile-example)
