

### TODO LIST

1. How to install the jenkins on the machine
2. Create the steps for CI workflow 
3. Create the steps for CD workflow
4. How to instlal the certificates on server?
5. How to install certificate in the jenkins instance?


How to install the jenkins on the machine?



	• https://support.sisense.com/hc/en-us/articles/360009441734-How-to-Export-a-Certificate-using-the-Certificate-Export-Wizard(How to export the certificates and assign the password)
	1.    Installed certificate on server(pfx) 
	2.       Changed pfx to jks for jenkins server 
	3.      keytool -importkeystore -srckeystore <path-to-cert-file.pfx> -srcstoretype pkcs12 -destkeystore jenkins.example.com.jks -deststoretype JKS 
	4.               10. Jenkins xml configuration changed  
	5. --httpPort=-1  (to stop Jenkins from listening over plain HTTP) 
	6. --httpsPort=443  (or 8443 or whatever SSL port you want Jenkins to listen on) 
	7. --httpsKeyStore="%JENKINS_HOME%(Giove the local Path)\jenkins.example.com.jks" 
	8. --httpsKeyStorePassword="<cleartext-password-to-keystore>" 
	 
	9. Pfx Certificate Password:Welc0me 
	 
	
	From <https://onenote.officeapps.live.com/o/onenoteframe.aspx?ui=en-US&hid=e06ae720-d0ac-48bd-bf12-3ca85ccb7b85&wopisrc=https%3A%2F%2Fcmog.sharepoint.com%2Fsites%2FDreamIT%2F_vti_bin%2Fwopi.ashx%2Ffolders%2F0716eb92061f43cd98cb2784d1391c54&wdorigin=teams&embed=1&embedded=1&removeshareui=1&hideheader=0&hidepagesearch=1&disablefile=0&disableview=0&disableprint=1&disablechat=1&jsapi=1&newsession=1&corrid=340f008a-0bb2-4a53-bed2-a5390608f6a8&usid=340f008a-0bb2-4a53-bed2-a5390608f6a8&wdredirectionreason=Unified_NotEditOrViewActionUrl> 
	
	
How to put change certificate format and give JAVA_HOME to local path in the keystore parameter
https://wiki.jenkins.io/pages/viewpage.action?pageId=135468777







node {
    stage('clone')
    {
      git 'https://github.com/kaviproject/TestJenkin_VS2019'
    }

    stage('Restore NUget Packages ')
    {
      bat label: '', script: '''"C:\\Users\\ParupatiK\\tools\\nuget.exe" restore "C:\\Program Files (x86)\\Jenkins\\workspace\\POCPIPELINE\\TestJenkin_VS2019.sln" 
'''
    }
    stage('BUILD')
    {
  
     bat "\"${tool 'MSBuild'}\\msbuild.exe\" TestJenkin_VS2019.sln /p:Configuration=Release /t:clean /t:package /p:FilesToIncludeForPublish=AllFilesInProjectFolder"
    }
    stage('Testing')
    {
        echo 'Testing results'
    }
    stage('Deploy')
    {
        bat label: '', script: 'C:\\"Program Files (x86)"\\IIS\\"Microsoft Web Deploy V3"\\msdeploy.exe -verb:sync -source:package="C:\\Program Files (x86)\\Jenkins\\workspace\\POCPIPELINE\\TestJenkin_VS2019\\obj\\Release\\Package\\TestJenkin_VS2019.zip" -dest:auto,computername=\'xxxxx\',username=\'xxxx\',password=\'xxxx\' -setParam:name="IIS Web Application Name",value="Default Web Site\\ParupatikComputerCDDeploy"'
    }
}


<b>Kubernates Link:</b>
https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app
https://medium.com/containerum/configuring-ci-cd-on-kubernetes-with-jenkins-89eab7234270


Userful Docker links:
https://www.freecodecamp.org/news/docker-101-fundamentals-and-practice-edb047b71a51/
https://djangostars.com/blog/what-is-docker-and-how-to-use-it-with-python/


