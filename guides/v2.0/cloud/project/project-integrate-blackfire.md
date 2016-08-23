---
layout: default
group: cloud
subgroup: 07_project
title: Blackfire integration
menu_title: Blackfire integration
menu_order: 105
menu_node: 
level3_menu_node: level3child
level3_subgroup: integrate
version: 2.0
github_link: cloud/project/project-integrate-blackfire.md
---

#### Contents
*	[Get your Blackfire credentials](#cloud-int-black) 
*	[Enable the Blackfire integration](#cloud-int-black-en)
*	[Verify it's working](#cloud-int-black-verify)
*	[Profile your site](#cloud-int-black-profile)

## Get your Blackfire credentials {#cloud-int-black}
Your project includes Blackfire credentials, which are available to owner of the Magento Enterprise Cloud Edition.

To get your Blackfire credentials:

1.	As the Magento Enterprise Cloud Edition account owner, [log in]({{ page.baseurl }}cloud/project/project-webint-basic.html#project-login) to your Magento Enterprise Cloud Edition project.
2.	In the upper right corner, click **&lt;your name>** > **Account Settings** as the following figure shows.

	![Go to account settings]({{ site.baseurl }}common/images/cloud_acct-settings-option.png){:width="650px"}
3.	On your account page, click **View Details** for your project as the following figure shows.

	![View your project details]({{ site.baseurl }}common/images/cloud_blackfire-edit-details.png)
4.	On your project details page, expand **Blackfire**.

	Your Blackfire credentials display similar to the following.

	![Your Blackfire credentials]({{ site.baseurl }}common/images/cloud_blackfire-account-info.png){:width="450px"}

## Enable the Blackfire integration {#cloud-int-black-en}
We recommend you enable the Blackfire integration in an environment (that is, branch). You can optionally merge the environment with its parent after you know it's working.

To enable Blackfire:

1.	Log in to the server on which your SSH keys are stored.
1.	Log in to your Magento Enterprise Cloud Edition project.

		magento-cloud login
2.	List projects:

		magento-cloud project:list
3.	List environments in the project:

		magento-cloud environment:list -p <project ID>
4.	See what branch you're currently in, if any.

		git branch
5.	If necessary, check out an existing branch:

		magento-cloud environment:checkout <environment ID>

	You can also create a new branch using the `magento-cloud environment:branch` command.
6.	Back up the environment using a snapshot:

		magento-cloud snapshot:create -e <environment ID>
6.	Use a text editor to open `<project root dir>/.magento.app.yaml`.
7.	Add the following to the `extensions` block under `runtime:`

		    - name: blackfire
		      configuration:
		         server_id: "<blackfire Server ID>"
		         server_token: "<blackfire Server token>"

	Change `<blackfire Server ID>` and `<blackfire Server token>` to the values from your Blackfire account.

	For example:

		runtime:
    	  extensions:
    	    - mcrypt
		    - redis
		    - xsl
		    - json
		    - name: blackfire
		      configuration:
		         server_id: "<blackfire Server ID>"
		         server_token: "<blackfire Server token>"
8.	Save your changes to `.magento.app.yaml` and exit the text editor.
9.	Add, commit, and push your changes to the environment:

		git add -A
		git commit -m "<message>"
		git push origin

	If errors display during deployment, open `.magento.app.yaml` and check the syntax. Check indentation and spelling and try again.

## Verify it's working {#cloud-int-black-verify}
To verify Blackfire works, you can SSH to the environment and run a command as follows:

1.	Log in to the Web UI.
2.	In the right pane, click the name of your environment.
3. 	Hover the mouse pointer over **Access site**.
4.	Copy the **SSH access** URL to the clipboard as the following figure shows.

	![Find the SSH URL for the environment]({{ site.baseurl }}common/images/cloud_ssh-access2.png){:width="500px"}
5.	Enter that command in your terminal window to log in using SSH.
6.	Enter the following command:

		php --ri blackfire

	A sample result follows:

		blackfire

		Blackfire => enabled
		Blackfire => 1.10.3
		Timing measurement => cgt
		Num of CPU => 8
		Profiling heap memory => 0 Kb
		Main instance trigger mode => HTTP header triggered
		Main instance => enabled

        Main instance info
		Output stream => file
		Signature validated => no
		EnvId validated => no
		Fully decoded => no

		Directive => Local Value => Master Value
		blackfire.agent_socket => tcp://blackfire.platform.sh:8307 => tcp://blackfire.platform.sh:8307
		blackfire.agent_timeout => 10 => 10
		blackfire.env_id => no value => no value
		blackfire.env_token => no value => no value
		blackfire.log_level => 1 => 1
		blackfire.log_file => no value => no value

## Profile your site {#cloud-int-black-profile}
To start profiling your site using Blackfire:

1.	Use the Web UI to find the **Web access** URL for your environment as the following figure shows:

	![Find your environment's URL]({{ site.baseurl }}common/images/cloud_web-access.png){:width="500px"}
2.	Go to your site using Chrome.
3.	In the upper right corner of the Chrome window, click ![Start Blackfire]({{ site.baseurl }}common/images/cloud_blackfire-logo.png).
4.	Follow the prompts on your screen to set up a comparison, then click **Profile!** as the following figure shows.

	![Start profiling]({{ site.baseurl }}common/images/cloud_blackfire.png)

Using Blackfire is beyond the scope of this guide. For more information, see one of the following resources:

*	[Profiling HTTP requests](https://blackfire.io/docs/cookbooks/profiling-http){:target="_blank"}
*	[Reference Guide](https://blackfire.io/docs/reference-guide/index){:target="_blank"}

#### Related topics
*	[Get started with a project]({{page.baseurl}}cloud/project/project-start.html)
*	[`.magento.app.yaml`]({{page.baseurl}}cloud/project/project-conf-files_magento-app.html)
