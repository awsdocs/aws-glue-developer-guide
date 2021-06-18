# Creating a Notebook Server Associated with a Development Endpoint<a name="dev-endpoint-notebook-server-considerations"></a>

One method for testing your ETL code is to use an Apache Zeppelin notebook running on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. When you use AWS Glue to create a notebook server on an Amazon EC2 instance, there are several actions you must take to set up your environment securely\. The development endpoint is built to be accessed from a single client\. To simplify your setup, start by creating a development endpoint that is used from a notebook server on Amazon EC2\.  

The following sections explain some of the choices to make and the actions to take to create a notebook server securely\. These instructions perform the following tasks: 
+ Create a development endpoint\.
+ Spin up a notebook server on an Amazon EC2 instance\.
+ Securely connect a notebook server to a development endpoint\.
+ Securely connect a web browser to a notebook server\.

## Choices on the AWS Glue Console<a name="dev-endpoint-notebook-server-console-actions"></a>

 For more information about managing development endpoints using the AWS Glue console, see [Viewing Development Endpoint Properties](console-development-endpoint.md)\. 

**To create the development endpoint and notebook server**

1. Sign in to the AWS Management Console and open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. Choose **Dev endpoints** in the navigation pane, and then choose **Add endpoint** to create a development endpoint\.

1. Follow the steps in the wizard to create a development endpoint that you plan to associate with one notebook server running on Amazon EC2\.

   In the **Add SSH public key \(optional\)** step, leave the public key empty\. In a later step, you generate and push a public key to the development endpoint and a corresponding private key to the Amazon EC2 instance that is running the notebook server\. 

1. When the development endpoint is provisioned, continue with the steps to create a notebook server on Amazon EC2\. On the development endpoints list page, choose the development endpoint that you just created\. Choose **Action**, **Create Zeppelin notebook server**, and fill in the information about your notebook server\. \(For more information, see [Viewing Development Endpoint Properties](console-development-endpoint.md)\.\)

1. Choose **Finish**\. The notebook server is created with an AWS CloudFormation stack\. The AWS Glue console provides you with the information you need to access the Amazon EC2 instance\.

   After the notebook server is ready, you must run a script on the Amazon EC2 instance to complete the setup\. 

## Actions on the Amazon EC2 Instance to Set Up Access<a name="dev-endpoint-notebook-server-ec2-actions"></a>

After you create the development endpoint and notebook server, complete the following actions to set up the Amazon EC2 instance for your notebook server\.

**To set up access to the notebook server**

1. If your local desktop is running Windows, you need a way to run commands SSH and SCP to interact with the Amazon EC2 instance\. You can find instructions for connecting in the Amazon EC2 documentation\. For more information, see [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)\. 

1. You can connect to your Zeppelin notebook using an HTTPS URL\. This requires a Secure Sockets Layer \(SSL\) certificate on your Amazon EC2 instance\. The notebook server must provide web browsers with a certificate to validate its authenticity and to allow encrypted traffic for sensitive data such as passwords\.  

   **If you have an SSL certificate from a certificate authority \(CA\)**, copy your SSL certificate key store onto the Amazon EC2 instance into a path that the `ec2-user` has write access to, such as `/home/ec2-user/`\. See the AWS Glue console notebook server details for the scp command to **Copy certificate**\. For example, open a terminal window, and enter the following command:

   ```
   scp -i ec2-private-key keystore.jks ec2-user@dns-address-of-ec2-instance:~/keystore.jks
   ```

    The truststore, *`keystore.jks`*, that is copied to the Amazon EC2 instance must have been created with a password\.

   The *ec2\-private\-key* is the key needed to access the Amazon EC2 instance\. When you created the notebook server, you provided an Amazon EC2 key pair and saved this EC2 private key to your local machine\. You might need to edit the **Copy certificate** command to point to the key file on your local machine\. You can also find this key file name on the Amazon EC2 console details for your notebook server\. 

   The *dns\-address\-of\-ec2\-instance* is the address of the Amazon EC2 instance where the keystore is copied\. 
**Note**  
There are many ways to generate an SSL certificate\. It is a security best practice to use a certificate generated with a certificate authority \(CA\)\. You might need to enlist the help of an administrator in your organization to obtain the certificate\. Follow the policies of your organization when you create a keystore for the notebook server\. For more information, see [Certificate authority](https://en.wikipedia.org/wiki/Certificate_authority) in Wikipedia\. 

   Another method is to generate a self\-signed certificate with a script on your notebook server Amazon EC2 instance\. However, with this method, each local machine that connects to the notebook server must be configured to trust the certificate generated before connecting to the notebook server\. Also, when the generated certificate expires, a new certificate must be generated and trusted on all local machines\. For more information about the setup, see [Self\-signed certificates](#dev-endpoint-notebook-server-self-signed-certificate)\. For more information, see [Self\-signed certificate](https://en.wikipedia.org/wiki/Self-signed_certificate) in Wikipedia\. 

1. Using SSH, connect to the Amazon EC2 instance that is running your notebook server; for example: 

   ```
   ssh -i ec2-private-key ec2-user@dns-address-of-ec2-instance
   ```

   The *ec2\-private\-key* is the key that is needed to access the Amazon EC2 instance\. When you created the notebook server, you provided an Amazon EC2 key pair and saved this EC2 private key to your local machine\. You might need to edit the **Copy certificate** command to point to the key file on your local machine\. You can also find this key file name on the Amazon EC2 console details for your notebook server\. 

   The *dns\-address\-of\-ec2\-instance* is the address of the Amazon EC2 instance where the keystore is copied\. 

1. From the home directory, `/home/ec2-user/`, run the `./setup_notebook_server.py` script\. AWS Glue created and placed this script on the Amazon EC2 instance\. The script performs the following actions:
   + **Asks for a Zeppelin notebook password:** The password is SHA\-256 hashed plus salted\-and\-iterated with a random 128\-bit salt kept in the `shiro.ini` file with restricted access\. This is the best practice available to Apache Shiro, the authorization package that Apache Zeppelin uses\.
   + **Generates SSH public and private keys:** The script overwrites any existing SSH public key on the development endpoint that is associated with the notebook server\. **As a result, any other notebook servers, Read–Eval–Print Loops \(REPLs\), or IDEs that connect to this development endpoint can no longer connect\.**
   + **Verifies or generates an SSL certificate:** Either use an SSL certificate that was generated with a certificate authority \(CA\) or generate a certificate with this script\. If you copied a certificate, the script asks for the location of the keystore file\. Provide the entire path on the Amazon EC2 instance, for example, `/home/ec2-user/keystore.jks`\. The SSL certificate is verified\.

   The following example output of the `setup_notebook_server.py` script generates a self\-signed SSL certificate\.

   ```
   Starting notebook server setup. See AWS Glue documentation for more details.
   Press Enter to continue...
   
   
   Creating password for Zeppelin user admin
   Type the password required to access your Zeppelin notebook:
   Confirm password:
   Updating user credentials for Zeppelin user admin
   
   Zeppelin username and password saved.
   
   
   Setting up SSH tunnel to devEndpoint for notebook connection.
   Do you want a SSH key pair to be generated on the instance? WARNING this will replace any existing public key on the DevEndpoint [y/n] y
   Generating SSH key pair /home/ec2-user/dev.pem
   Generating public/private rsa key pair.
   Your identification has been saved in /home/ec2-user/dev.pem.
   Your public key has been saved in /home/ec2-user/dev.pem.pub.
   The key fingerprint is:
   26:d2:71:74:b8:91:48:06:e8:04:55:ee:a8:af:02:22 ec2-user@ip-10-0-0-142
   The key's randomart image is:
   +--[ RSA 2048]----+
   |.o.oooo..o.      |
   |  o. ...+.       |
   | o  . . .o       |
   |  .o . o.        |
   |  . o o S        |
   |E.   . o         |
   |=                |
   |..               |
   |o..              |
   +-----------------+
   
   Attempting to reach AWS Glue to update DevEndpoint's public key. This might take a while.
   Waiting for DevEndpoint update to complete...
   Waiting for DevEndpoint update to complete...
   Waiting for DevEndpoint update to complete...
   DevEndpoint updated to use the public key generated.
   Configuring Zeppelin server...
   
   ********************
   We will configure Zeppelin to be a HTTPS server. You can upload a CA signed certificate for the server to consume (recommended). Or you can choose to have a self-signed certificate created.
   See AWS Glue documentation for additional information on using SSL/TLS certificates.
   ********************
   
   Do you have a JKS keystore to encrypt HTTPS requests? If not, a self-signed certificate will be generated. [y/n] n
   Generating self-signed SSL/TLS certificate at /home/ec2-user/ec2-192-0-2-0.compute-1.amazonaws.com.jks
   Self-signed certificates successfully generated.
   Exporting the public key certificate to /home/ec2-user/ec2-192-0-2-0.compute-1.amazonaws.com.der
   Certificate stored in file /home/ec2-user/ec2-192-0-2-0.compute-1.amazonaws.com.der
   Configuring Zeppelin to use the keystore for SSL connection...
   
   Zeppelin server is now configured to use SSL.
   SHA256 Fingerprint=53:39:12:0A:2B:A5:4A:37:07:A0:33:34:15:B7:2B:6F:ED:35:59:01:B9:43:AF:B9:50:55:E4:A2:8B:3B:59:E6
   
   
   **********
   The public key certificate is exported to /home/ec2-user/ec2-192-0-2-0.compute-1.amazonaws.com.der
   The SHA-256 fingerprint for the certificate is 53:39:12:0A:2B:A5:4A:37:07:A0:33:34:15:B7:2B:6F:ED:35:59:01:B9:43:AF:B9:50:55:E4:A2:8B:3B:59:E6.
   You may need it when importing the certificate to the client. See AWS Glue documentation for more details.
   **********
   
   Press Enter to continue...
   
   
   All settings done!
   
   
   Starting SSH tunnel and Zeppelin...
   autossh start/running, process 6074
   Done. Notebook server setup is complete. Notebook server is ready.
   See /home/ec2-user/zeppelin/logs/ for Zeppelin log files.
   ```

1. Check for errors with trying to start the Zeppelin server in the log files located at `/home/ec2-user/zeppelin/logs/`\.

## Actions on Your Local Computer to Connect to the Zeppelin Server<a name="dev-endpoint-notebook-server-local-actions"></a>

After you create the development endpoint and notebook server, connect to your Zeppelin notebook\. Depending on how you set up your environment, you can connect in one of the following ways\.

1. **Connect with a trusted CA certificate\.** If you provided an SSL certificate from a certificate authority \(CA\) when the Zeppelin server was set up on the Amazon EC2 instance, choose this method\. To connect with HTTPS on port 443, open a web browser and enter the URL for the notebook server\. You can find this URL on the development notebook details page for your notebook server\. Enter the contents of the **HTTPS URL** field; for example:

   ```
   https://public-dns-address-of-ec2-instance:443 
   ```

1. <a name="dev-endpoint-notebook-server-self-signed-certificate"></a>**Connect with a self\-signed certificate\.** If you ran the `setup_notebook_server.py` script to generate an SSL certificate, first trust the connection between your web browser and the notebook server\. The details of this action vary by operating system and web browser\. The general work flow is as follows:

   1. Access the SSL certificate from the local computer\. For some scenarios, this requires you to copy the SSL certificate from the Amazon EC2 instance to the local computer; for example:

      ```
        scp -i path-to-ec2-private-key ec2-user@notebook-server-dns:/home/ec2-user/notebook-server-dns.der  notebook-server-dns.der
      ```

   1. Import and view \(or view and then import\) the certificate into the certificate manager that is used by your operating system and browser\. Verify that it matches the certificate generated on the Amazon EC2 instance\.

   **Mozilla Firefox browser:**

   In Firefox, you might encounter an error like **Your connection is not secure**\. To set up the connection, the general steps are as follows \(the steps might vary by Firefox version\):

   1. Find the **Options** or **Preferences** page, navigate to the page and choose **View Certificates**\. This option might appear in the **Privacy**, **Security**, or **Advanced** tab\.

   1. In the **Certificate Manager** window, choose the **Servers** tab, and then choose **Add Exception**\.

   1. Enter the HTTPS **Location** of the notebook server on Amazon EC2, and then choose **Get Certificate**\. Choose **View**\.

   1. Verify that the **Common Name \(CN\)** matches the DNS of the notebook server Amazon EC2 instance\. Also, verify that the **SHA\-256 Fingerprint** matches that of the certificate generated on the Amazon EC2 instance\. You can find the SHA\-256 fingerprint of the certificate in the output of the `setup_notebook_server.py` script or by running an openssl command on the notebook instance\.

      ```
        openssl x509 -noout -fingerprint -sha256 -inform der -in path-to-certificate.der
      ```

   1. If the values match, **confirm** to trust the certificate\.

   1. When the certificate expires, generate a new certificate on the Amazon EC2 instance and trust it on your local computer\.

   **Google Chrome browser on macOS:**

   When using Chrome on macOS, you might encounter an error like **Your connection is not private**\. To set up the connection, the general steps are as follows:

   1. Copy the SSL certificate from the Amazon EC2 instance to your local computer\.

   1. Choose **Preferences** or **Settings** to find the **Settings** page\. Navigate to the **Advanced** section, and then find the **Privacy and security** section\. Choose **Manage certificates**\.

   1. In the **Keychain Access** window, navigate to the **Certificates** and choose **File**, **Import items** to import the SSL certificate\.

   1. Verify that the **Common Name \(CN\)** matches the DNS of the notebook server Amazon EC2 instance\. Also, verify that the **SHA\-256 Fingerprint** matches that of the certificate generated on the Amazon EC2 instance\. You can find the SHA\-256 fingerprint of the certificate in the output of the `setup_notebook_server.py` script or by running an openssl command on the notebook instance\.

      ```
        openssl x509 -noout -fingerprint -sha256 -inform der -in path-to-certificate.der
      ```

   1. Trust the certificate by setting **Always Trust**\.

   1. When the certificate expires, generate a new certificate on the Amazon EC2 instance and trust it on your local computer\.

   **Chrome browser on Windows:**

   When using Chrome on Windows, you might encounter an error like **Your connection is not private**\. To set up the connection, the general steps are as follows:

   1. Copy the SSL certificate from the Amazon EC2 instance to your local computer\.

   1. Find the **Settings** page, navigate to the **Advanced** section, and then find the **Privacy and security** section\. Choose **Manage certificates**\.

   1. In the **Certificates** window, navigate to the **Trusted Root Certification Authorities** tab, and choose **Import** to import the SSL certificate\.

   1. Place the certificate in the **Certificate store** for **Trusted Root Certification Authorities**\.

   1. Trust by installing the certificate\.

   1. Verify that the **SHA\-1 Thumbprint** that is displayed by the certificate in the browser matches that of the certificate generated on the Amazon EC2 instance\. To find the certificate on the browser, navigate to the list of **Trusted Root Certification Authorities**, and choose the certificate **Issued To** the Amazon EC2 instance\. Choose to **View** the certificate, choose **Details**, and then view the **Thumbprint** for `sha1`\. You can find the corresponding SHA\-1 fingerprint of the certificate by running an `openssl` command on the Amazon EC2 instance\.

      ```
        openssl x509 -noout -fingerprint -sha1 -inform der -in path-to-certificate.der
      ```

   1. When the certificate expires, generate a new certificate on the Amazon EC2 instance and trust it on your local computer\.

   **Microsoft Internet Explorer browser on Windows:**

   When using Internet Explorer on Windows, you might encounter an error like **Your connection is not private**\. To set up the connection, the general steps are as follows:

   1. Copy the SSL certificate from the Amazon EC2 instance to your local computer\.

   1. Find the **Internet Options** page, navigate to the **Content** tab, and then find the **Certificates** section\. 

   1. In the **Certificates** window, navigate to the **Trusted Root Certification Authorities** tab, and choose **Import** to import the SSL certificate\.

   1. Place the certificate in the **Certificate store** for **Trusted Root Certification Authorities**\.

   1. Trust by installing the certificate\.

   1. Verify that the **SHA\-1 Thumbprint** that is displayed by the certificate in the browser matches that of the certificate generated on the Amazon EC2 instance\. To find the certificate on the browser, navigate to the list of **Trusted Root Certification Authorities**, and choose the certificate **Issued To** the Amazon EC2 instance\. Choose to **View** the certificate, choose **Details**, and then view the **Thumbprint** for `sha1`\. You can find the corresponding SHA\-1 fingerprint of the certificate by running an openssl command on the Amazon EC2 instance\.

      ```
        openssl x509 -noout -fingerprint -sha1 -inform der -in path-to-certificate.der
      ```

   1. When the certificate expires, generate a new certificate on the Amazon EC2 instance and trust it on your local computer\.

   After you trust the certificate, to connect with HTTPS on port 443, open a web browser and enter the URL for the notebook server\. You can find this URL on the development notebook details page for your notebook server\. Enter the contents of the **HTTPS URL** field; for example:

   ```
   https://public-dns-address-of-ec2-instance:443 
   ```