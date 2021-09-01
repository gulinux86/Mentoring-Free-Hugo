[![CI](https://github.com/SinesioBittencourt-StudyGuide/HugoTest/actions/workflows/deploy.yml/badge.svg)](https://github.com/SinesioBittencourt-StudyGuide/HugoTest/actions/workflows/deploy.yml) 

# Continuously Deploy Hugo with GitHub Actions and LetsCloud

Post Original:
https://www.letscloud.io/community/continuously-deploy-hugo-with-github-actions-docker-and-letscloud

[![LinkedIn Badge](https://img.shields.io/badge/LinkedIn-Profile-informational?style=flat&logo=linkedin&logoColor=white&color=0D76A8)](https://www.linkedin.com/in/sinesiobittencourt/)
---

## Introduction

  In this tutorial, I will simply explain my solution to a problem I had. If you have any suggestions, feel free to share them with me!

  To create this blog, I had a few limitations:

  - I wanted to write the posts using Markdown;
  - I wanted to publish the posts without using a dedicated administration site (like the one I have if I use WordPress);
  - I wanted to use my VPS on LetsCloud;

  To solve my problem, I decided to use [Hugo](https://gohugo.io/), a generator of static open source websites, GitHub Actions and Docker on my VPS.

  We will see how to generate our site in Hugo and deploy it continuously using GitHub Actions and Docker.

  

  ### Related Articles

  [how-to-deploy-a-go-web-application-using-nginx-on-ubuntu-1904](https://www.letscloud.io/community/how-to-deploy-a-go-web-application-using-nginx-on-ubuntu-1904) 
<br>
  [how-to-install-and-use-docker-on-ubuntu-1804](https://www.letscloud.io/community/how-to-install-and-use-docker-on-ubuntu-1804)
	<br>
	[how-to-install-golang-on-ubuntu-2004-or-2010](https://www.letscloud.io/community/how-to-install-golang-on-ubuntu-2004-or-2010)

  So let's take a look at how it all works?

  ### Prerequisites

  - Ubuntu 20.04 or Ubuntu 20.10 
  - 1024MB or above Ram.
  - 20GB Disk Space.
  - 1 vCPU or above CPU.
  - Internet connection to download Terraform
  - root privileges

## Step 1 - First of all, you have to install Hugo

::: info
[here you can find all the information's about that.](https://gohugo.io/getting-started/installing/)
:::

After this first step you can create a new folder on your computer and then you can create a new Hugo project into this folder:

```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud  
╰─➤  mkdir HugoTest
```
```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud  
╰─➤  cd HugoTest  
```
```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud/HugoTest  
╰─➤  hugo new site HugoTest

Congratulations! Your new Hugo site is created in /home/sinesio/Desktop/LetsCloud/HugoTest/HugoTest.

Just a few more steps and you're ready to go:
1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "Hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files with "Hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".
```

## Step 2 - If everything goes well you will find a new folder containing some folders like “archetypes”, “data” and “content”

Some of the folders that Hugo created are empty, you have to create a “.gitignore” file in each of the empty folders.
The “.gitignore” must contain the following text:

```super_user
!.gitignore
```

## Step 3 - Now you can create a new GitHub repository.

![image1](https://cm-static.letscloud.io/community/186/1png.png)
<br>
![image2](https://cm-static.letscloud.io/community/186/2png.png)

## Step 4 - Now you can follow the setup provided by GitHub when you create the new repo

 Move in the Hugo project folder and, using your terminal write the following commands:

```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud/HugoTest/HugoTest  
╰─➤  git init
Initialized empty Git repository in /home/sinesio/Desktop/LetsCloud/HugoTest/HugoTest/.git/
```

```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud/HugoTest/HugoTest  ‹master*› 
╰─➤  git add .
```

```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud/HugoTest/HugoTest  ‹master*› 
╰─➤  git commit -m "First commit"
:::output
[master (root-commit) b9e3274] First commit
 3 files changed, 9 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 archetypes/default.md
 create mode 100644 config.toml
 :::
```

```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud/HugoTest/HugoTest  ‹master› 
╰─➤  git branch -M main
```

```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud/HugoTest/HugoTest  ‹main› 
╰─➤  git remote add origin https://github.com/SinesioBittencourt-StudyGuide/HugoTest.git
```

```super_user
╭─sinesio@Linux ~/Desktop/LetsCloud/HugoTest/HugoTest  ‹main› 
╰─➤  git push -u origin main
:::output
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 507 bytes | 507.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To https://github.com/SinesioBittencourt-StudyGuide/HugoTest.git
[new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
:::
```

You should see this in your GitHub:

![image 17](https://cm-static.letscloud.io/community/186/image-20210504191934332png.png)

## Step 5 - Set up LetsCloud

We can now setup the synchronization with the VPS using GitHub Actions. Let’s move to our VPS and create a new folder for this project.

![image3](https://cm-static.letscloud.io/community/186/3png.png)

Where do you want to deploy and select an image?

![image4](https://cm-static.letscloud.io/community/186/4png.png)

Choose your resources

![image5](https://cm-static.letscloud.io/community/186/5png.png)

Set a Label and Hostname

![image 6](https://cm-static.letscloud.io/community/186/6png.png)

Check Status!

![image 7](https://cm-static.letscloud.io/community/186/7png.png)

## Step 6 - Now we can clone on the VPS the GitHub that we created

```super_user
git clone https://github.com/SinesioBittencourt-StudyGuide/hands-on-hugo.git
```

## Step 7 - Now you have to generate a secret token on GitHub

You’ll need this for the Action. 

::: info
You have to go to your’s profile:
Setting > “Developer Setting” > “Personal Access Tokens” > "Generate new token"
:::
::: info
You have to copy the generated code, you will need it for the next step.
:::

![image 8](https://cm-static.letscloud.io/community/188/8png.png)

![image 9](https://cm-static.letscloud.io/community/188/9png.png)

## Step 8 - Now we can add some secret informations to our repository

Go to the “Settings” tab and then to “Secrets”

![image 10](https://cm-static.letscloud.io/community/188/10png.png)

## Step 9 - We have to add some secrets parameters that we will use in our GitHub Action

Click in “New Secret” and add the following parameters:
::: info
PROJECT_PATH
:::
PROJECT_PATH is the path of the repo that you cloned on your VPS, in my case is /home/TestHugo
::: info
SERVER_IP
:::
The IP address of your VPS
::: info
SERVER_PASSWORD
:::
The VPS's password
::: info
SERVER_USERNAME
:::
The username that you use to login into your VPS
::: info
TOKEN 
:::

## Step 10 - The token you generated in the previous step

Now we can setup our GitHub Action, you have to go to the page of your repo and then click on the “Actions” button, then you have to click on “Set up a workflow yourself”.

In the Action’s editor add the following code:

    https://github.com/SinesioBittencourt-StudyGuide/hands-on-hugo/blob/main/.github/workflows/deploy.yml

Commit the Action by clicking on “Start commit” and then to “Commit new file”.

![image 11](https://cm-static.letscloud.io/community/186/11png.png)

If you go to the Actions tab you will see the actions that are running and if everything is ok you will see something like this:

![image 12](https://cm-static.letscloud.io/community/186/12png.png)

![image 13](https://cm-static.letscloud.io/community/186/13png.png)

## Step 11 - Now you have to choose the theme of your blog       

In this tutorial we will use Harbor, on the VPS you have to use the following commands:

```super_user
git submodule add https://github.com/matsuyoshi30/harbor.git ./themes/harborecho 'theme = "harbor"' >> config.toml
```

## Step 12 - Now we can set up the Dockerfile                    
In Hugo project’s folder you just have to create a file called “Dockerfile”. Paste the following lines in the “Dockerfile”:

```
https://github.com/SinesioBittencourt-StudyGuide/hands-on-hugo/blob/main/.github/workflows/deploy.yml
```

## Step 13 - Create the file docker-compose.
In the same folder you have to create the file “docdocker-composeker-compose.yml”:

```
https://github.com/SinesioBittencourt-StudyGuide/hands-on-hugo/blob/main/docker-compose.yml
```

## Step 14 - Now you can launch your docker container using the command

```super_user
docker-compose up 
```

If everything is **ok** you should see something like this:

![image 20](https://cm-static.letscloud.io/community/189/20png.png)

## Step 15 - Connect to the IP of your VPS

If you connect to the IP of your VPS using port 1313 you should also see your newly created blog:

![image 21](https://cm-static.letscloud.io/community/189/21png.png)

## Step 16 -  Now you have to push the new file to Github.

The actions that you have created in one of the previous steps will start running and if everything is ok you will see this:

![image 14](https://cm-static.letscloud.io/community/186/14png.png)

## Step 17 - using the -d option in docker-compose 

Now on the VPS we can kill the “docker-compose up” command and if we want to maintain online our website even if we close the connection to the VPS we can launch it again using the -d option:

```super_user
docker-compose up -d
```

## Conclusions

That’s all for today, I hope that this tutorial was useful for you, if so, clap your hands!

GitHub repo: 	[hands-on-hugo](https://github.com/SinesioBittencourt-StudyGuide/hands-on-hugo)

If you have any suggestions feel free to contact me or just leave a comment below.

