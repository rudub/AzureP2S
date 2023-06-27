# AzureP2S
Azure point to site connectivity

The aim of creating this lab is to understand and test how you can connect to infrastructure vent resources from one specific machine or few machines.

P2S works on certification and hence you need a root certificate which will use for authentication form azure and client certificate which is required for client machine authentication.

1. Create a VNET with address space : 172.16.0.0/16
2. Create a subnet : 172.16.1.0/24
3. Create a Gateway subnet : 172.16.200.0/24
4. Create a Virtual network gateway (Route based).
5. Create s self signed certificate by using below command:
6. $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature ` -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable ` -HashAlgorithm sha256 -KeyLength 2048 ` -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign 
Point-to-Site connections require the certificate public key .cer file (not the private key) to be uploaded to Azure.
1. Export the certificate to .cer file from certificate manager by using below steps:
2. To obtain a .cer file from the certificate, open Manage user certificates. Locate the self-signed root certificate, typically in 'Certificates - Current User\Personal\Certificates', and right-click. Click All Tasks, and then click Export. This opens the Certificate Export Wizard.
3. In the Wizard, click Next. Select No, do not export the private key, and then click Next.
4. On the Export File Format page, select Base-64 encoded X.509 (.CER)., and then click Next.
5. On the File to Export, Browse to the location to which you want to export the certificate. For File name, name the certificate file. Then, click Next.
6. Click Finish to export the certificate. You see The export was successful. Click OK to close the wizard.


Next once the VON gateway created add the client address pool and certificate on azure. 
1. Navigate to Virtual Network gateway and go to point to site connection.
2. Add client address pool (local machine which you want to have access to servers and resources)
3. Open the certificate file in notepad and copy the content.
4. Add that content into azure portal in certificate section and create root certificate and save the configuration.
5. If you want to connect to server other then the machine you configure P2S you need to install client certificate. Follow below steps for the same:
1. To export a client certificate, open Manage user certificates. The client certificates that you generated are, by default, located in 'Certificates - Current User\Personal\Certificates'. Right-click the client certificate that you want to export, click all tasks, and then click Export to open the Certificate Export Wizard.
2. In the Wizard, click Next, then select Yes, export the private key, and then click Next.
3. On the Export File Format page, leave the defaults selected. Make sure that Include all certificates in the certification path if possible is selected. Selecting this also exports the root certificate information that is required for successful authentication. Then, click Next.
4. On the Security page, you must protect the private key. If you select to use a password, make sure to record or remember the password that you set for this certificate. Then, click Next.
5. On the File to Export, Browse to the location to which you want to export the certificate. For File name, name the certificate file. Then, click Next.
6. Click Finish to export the certificate.
1. After that install the VPN from azure portal and connect to  your resources.  
