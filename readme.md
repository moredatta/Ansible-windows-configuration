Connect Ansible on Windows from Ubuntu..


1.Install ansible and configure on ubuntu (Refer Link):
	https://geekflare.com/ansible-installation-ubuntu/

2.Install ansible  on Windows: (Refer Link)
	https://geekflare.com/ansible-installation-windows/

3. Create Ansible user windows.
	.Open Computer Management on your Windows system and go to Local Users and Groups.
	.Right-click on Users and create a new user.
	.Select Password never expires checkbox and click on create.
4.User group permission.
	.Now among the available groups, right-click on the Administrators group and click on properties.
	.Click on Add and enter ansible in object names.
	.Click on the check names option and then Ok.
5.Go to your ansible controller machine, update it, and install the libraries mentioned below.
	1.sudo apt-get update
	2.sudo apt-get install gcc python-dev
	3.sudo apt install python3-pip
6.Mention host in inventory file
	sudo nano /etc/ansible/hosts
7.Update the windows env .
	ansible_user: ansible

	ansible_password: ansible

	ansible_connection: winrm

	ansible_winrm_server_cert_validation: ignore

	ansible_winrm_transport: basic

	ansible_winrm_port: 5985

	ansible_python_interpreter: C:\Users\geekflare\AppData\Local\Programs\Python\Python37\python

8. Configure Windows Servers to Manage
	$url = "https://raw.githubusercontent.com/jborean93/ansible-windows/master/scripts/Upgrade-PowerShell.ps1"
	$file = "$env:temp\Upgrade-PowerShell.ps1"
	$username = "ansible"
	$password = "ansible"
	(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
	 Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force
	 &$file -Version 5.1 -Username $username -Password $password -Verbose
9.To configure WinRM on a Windows system with ansible, a remote configuration script has been provided by ansible. Run the script in the PowerShell.
	 $url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
	 $file = "$env:temp\ConfigureRemotingForAnsible.ps1"
	(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
	 powershell.exe -ExecutionPolicy ByPass -File $file
10.Set winrm to allow HTTP traffic.
	 winrm set winrm/config/service '@{AllowUnencrypted="true"}'
11.Set the authentication to basic in wirm.
	 winrm set winrm/config/service/auth '@{Basic="true"}'
12.Now all the steps on the machine are done. Go to ansible controller machine and ping the windows server machine using win_ping ansible module.
	 ansible win -m win_ping

