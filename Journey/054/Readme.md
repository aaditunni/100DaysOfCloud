# Remove AWS sensitive data from GitHub commit history

## Introduction

Sometimes we accidentally add our AWS sensitive data like account id, Access keys, etc in in GitHub as a string maybe in a json file or some sort and then when we realize it, we delete it from the GitHub. The problem is even though we delete it, It will be present in the commit history. To remove that from the commit history we use a tool called BFG Repo-Cleaner.

## Try yourself

### Step 1 — 
- Remove the sensitive data from the GitHub repo and commit.
- You can notice that even though you removed it, it will be there in the commit history.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/054/day54.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/054/day54.1.JPG)

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/054/day54.2.JPG)

- I am using GitPod as my environment to open my repo instead of cloning to local machine.
In GitPod workspace, install BFG Repo-Cleaner.
    ```
        brew install bfg
    ```
This is the command for mac. For other OS, see the picture below.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/054/day54.5.JPG)

- Once it is installed, create a text file named sensitive.txt and add all the sensitive data in it.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/054/day54.3.JPG)

- Move the text file one directory back. I created the text file by right clicking and selecting create a new file option and then moved it using this command :
    ```
        mv sensitive.txt ..
    ```
- Go back one directory. 
    ```
        cd ..
    ```
- Create a bare repo:
    ```
        git clone --mirror git_https_clone_url bare_repo_name
    ```
- Replace git_https_clone_url with your git https url and bare_repo_name with a name for your bare repo.
- This is a bare repo, which means your normal files won't be visible, but it is a full copy of the Git database of your repository, and at this point you should make a backup of it to ensure you don't lose anything.
- Move to the bare repo directory:
    ```
        cd bare_repo_name
    ```
- Replace all sensitive data listed in sensitive.txt wherever it can be found in the GitHub commit history:
    ```
        bfg --replace-text ../sensitive.txt
    ```
- Run:
    ```
        git reflog expire --expire=now --all && git gc --prune=now --aggressive 
    ```
Push it:
    ```
        git push
    ```
- It is always recommended to clone it and push it to your main. You might also want to backup the repo on your local machine as well.
- Now you can notice all the sensitive that we wanted to be removed are removed from the GitHub commit history and replaced with *** REMOVED *** .

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/054/day54.4.JPG)

## ☁️ Cloud Outcome

Removed AWS sensitive data from GitHub commit history.

## Social Proof

[Blog](https://dev.to/aaditunni/remove-aws-sensitive-data-from-github-commit-history-bip)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7034651929779580928-HFsE?utm_source=share&utm_medium=member_desktop)
