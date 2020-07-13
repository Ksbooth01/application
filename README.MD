GitHub/Jenkins setup insructions 

Pre-requisites
	You will need to know and replace the following with your relevant info:
	•	<Github_Account>
	•	<Github_Project>
	•	<YOUR_GITHUB_TOKEN> - generated during install
	•	<YOUR_JENKINS_URL>
	•	<JenkinsProjectName> – your Jenkins project name
		
What you will do in this setup:
GitHub Setup
	A. Add a Webhook for Jenkins
	B. Generate a GitHub access token
Jenkins Setup
	A.	Install GitHub plugin
	B.	Configure Jenkins for GitHub Credentials:
	C. Setup Jenkins Pipeline
	
GitHub Setup 
	A. Add a webhook
		What you’ll do here is install the Jenkins plugin on your GitHub repository in order to trigger a build in Jenkins when you commit a change in GitHub. 
		This way, Jenkins can run tests to make sure your code is working as expected.
		1.	In your browser, go to your GitHub account. The URL should be something like www.github.com/your_username
		2.	Go into the repository setup for this application
		3.	Click on Settings, on the upper right part of the page
		4.	Click on Webhooks
		5.	Click on Add Webhook
		6.	Set the payload url to http://<YOUR_JENKINS_URL>/github-webhook/
		7.	Set Content type to application/json
		8.	Select the webhook to be triggered by “Just the push event”.
		9.	Click on Add webhook
 	B. Generate a GitHub access token
		This token can be used by an application to use an API, here the application is Jenkins and the API is the GitHub API to update the Jenkins build status in the pull request.
		Ok, now you’re on to some non-obvious items.
		1. Go to your GitHub account.
		2. Under your profile, open the drop-down menu and click on Settings.
		3. Go to Developer settings on the left side menu, all the way on the bottom:
		4. Click on Personal access tokens:
		5. On the top right corner of the screen click on the Generate new token button.
		6. Insert your password.
		7. Under repo check the following checkboxes: 
			repo:status
			public_repo
			user
			gist
		8. Click on Generate token at the bottom of the page.
		9. Make sure to save your token somewhere so you can use it later. this is <YOUR_GITHUB_TOKEN>

Jenkins Setup  
	A. Install GitHub plugin:
		1.	In your browser, go to your Jenkins URL <YOUR_JENKINS_URL>
		2.	Click on Manage Jenkins
		3.	Click on Manage Plugins
		4.	Go to Available tab and search for GitHub integration plugin
		5.	Check the checkbox on the left and click on download and install after restart
	B. Configure Jenkins for GitHub Credentials: 
		1.	In your browser, go to your Jenkins URL
		2.	Click on Manage Jenkins
		3.	Click on Configure System
		4.  Under GitHub -> Github Server -> Credentials click Add
		5.	On Add Credentials under kind choose secret text, on ID paste <YOUR_GITHUB_TOKEN>, then click add
		6. 	Choose 'secret text' from credentials and then test connection.  Once the connection is validated 
		7.	Click save.
	C. Setup Jenkins Pipeline:
		1.	Go to your Jenkins URL <YOUR_JENKINS_URL>
		2.  Click New items
		3. 	Enter and item name enter <JenkinsProjectName>
		4.  Click Freestyle project
		5.	For Source Code Management select Git
		6.	For Repository URL enter https://github.com/<Github_Account>/<Github_Project>.git
		7.  In Build select Execute Shell and copy the following  in the command box
				terraform init
				terraform validate
				terraform plan -out=tfplan -input=false
				terraform apply tfplan
		8.	Go to Post-build Actions
		9.	Set build status on GitHub commit (universal)
		10.	Change Status result to One of the default messages and statuses
		11.	Save
    
