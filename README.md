# CI-CD-pipeline

<br><h1>Create a complete CI-CD pipeline using bash, python, and crontabs. HTML project, push it to a GitHub repository and update website after git repo update automatically </h1></br>
<br>==>> Create a simple HTML project and push it to a GitHub repository. </br>

![Website rajkumar4](https://github.com/user-attachments/assets/7777a62b-b3f7-4e99-b970-307d7ca365bd)

<br>==>> Create a repo named as CI-CD-pipeline</br>
<br>==>> Upload a index.html with below mention code.</br>
<br>==>> "<html> <b>Rajkumar Roy4 (Website) </b></html>"</br>
<br>==>> Set Up an AWS EC2 Linux Instance with Nginx</br>
<br>===>> Command "sudo apt-get install nginx"</br>
![iginx service status](https://github.com/user-attachments/assets/bcce3792-31e8-45f4-8f99-53f4d722b732)
<br>===>> mkdir /home/ubuntu/CI-CD-pipeline directory created for pull git repository (https://github.com/rajkumarroy007/CI-CD-pipeline.git)</br>
![Git clone Repo to local](https://github.com/user-attachments/assets/b8fea1e7-4d9f-48f7-82ff-3b2638696e03)
<br>===>> mkdir /home/ubuntu/CI-CD-pipeline/script dirctory create to update the python script to check new commets and shell script to update latest changes to iginx website location.</br>
<br>===>>update default path of nginx default configuration </br>
![nginx_default_fileupdate](https://github.com/user-attachments/assets/db7e1448-ea3b-4aad-abd9-91638eb3582b)

<br>===>> sudo nano mkdir /var/www/html/CI-CD-pipeline</br> given permission  </br>
<br>===>> sudo nano /etc/nginx/sites-enabled/default and update website path from /var/www/html to /var/www/html/CI-CD-pipeline</br>
![nginx_default_fileupdate](https://github.com/user-attachments/assets/db7e1448-ea3b-4aad-abd9-91638eb3582b)
<br>===>>sudo nano /home/ubuntu/CI-CD-pipeline/script/check_for_new_commits_github_api.py script created for new commit id to textfile last_known_commit_sha.txt</br>
<br>===>>sudo nano /home/ubuntu/CI-CD-pipeline/script/update_code_and_restart_nginx.sh  to update the latest change in website and restart nginx service</br>
<br>===>> crontab job created for shell and python script with below details  </br>
![crontab job](https://github.com/user-attachments/assets/4cfde01f-356b-4552-85ad-128b99b97e82)

<br>===>>    * * * * * sudo python3 /home/ubuntu/CI-CD-pipeline/script/check_for_new_commits_github_api.py  </br>
<br>===>>   * * * * * /home/ubuntu/CI-CD-pipeline/script/update_code_and_restart_nginx.sh  </br>
<br>===>>  Testing done by updating commit in repo and web url change automatically     </br>
![Edit Website html file and commit Rajkumar5](https://github.com/user-attachments/assets/ab19a852-1222-49f5-bf06-d04b2a0c9f37)
![Testing Website html file and commit Rajkumar5](https://github.com/user-attachments/assets/9b0809f0-a6d3-48e2-bd2d-ea5954518e13)


<br>===>> Python and bash script details </br>
###############################
<br>===>>check_for_new_commits_github_api.py  </br> 
<br>===>>last_known_commit_sha.txt   </br>
<br>===>>update_code_and_restart_nginx.sh </br>

![Python and bash script ](https://github.com/user-attachments/assets/6472bb54-28bc-42f6-925c-594504d81328)



<br>===>>check_for_new_commits_github_api.py  </br> 
######################################################
[code]
import requests
import json
import sys

# Constants
GITHUB_API_URL = "https://api.github.com"
REPO_OWNER = "username"  # Replace with the repository owner's username
REPO_NAME = "repository"  # Replace with the repository name
BRANCH_NAME = "main"  # Replace with the branch you want to check
ACCESS_TOKEN = "your_github_token"  # Replace with your GitHub Personal Access Token

# Function to get the latest commit hash from the remote branch using the GitHub API
def get_latest_commit_sha(owner, repo, branch, token):
    url = f"{GITHUB_API_URL}/repos/{owner}/{repo}/commits/{branch}"
    headers = {
        "Authorization": f"token {token}",
        "Accept": "application/vnd.github.v3+json"
    }
    response = requests.get(url, headers=headers)
    
    if response.status_code == 200:
        commit_data = response.json()
        return commit_data['sha']
    else:
        print(f"Failed to fetch commits from GitHub API. Status code: {response.status_code}")
        print(f"Response: {response.text}")
        sys.exit(1)

# Function to load the last known commit SHA from a file
def load_last_known_commit_sha(file_path):
    try:
        with open(file_path, "r") as file:
            return file.read().strip()
    except FileNotFoundError:
        return None

# Function to save the latest commit SHA to a file
def save_latest_commit_sha(file_path, sha):
    with open(file_path, "w") as file:
        file.write(sha)

# Main function to check for new commits
def check_for_new_commits():
    # File to store the last known commit SHA
    commit_sha_file = "last_known_commit_sha.txt"

    # Get the latest commit SHA from GitHub API
    latest_commit_sha = get_latest_commit_sha(REPO_OWNER, REPO_NAME, BRANCH_NAME, ACCESS_TOKEN)

    # Load the last known commit SHA from file
    last_known_commit_sha = load_last_known_commit_sha(commit_sha_file)

    # Check if there are new commits
    if last_known_commit_sha is None:
        print("No previous commit found. Saving the latest commit SHA.")
        save_latest_commit_sha(commit_sha_file, latest_commit_sha)
    elif latest_commit_sha != last_known_commit_sha:
        print("New commits detected!")
        print(f"Old Commit SHA: {last_known_commit_sha}")
        print(f"New Commit SHA: {latest_commit_sha}")
        # Update the stored commit SHA with the new one
        save_latest_commit_sha(commit_sha_file, latest_commit_sha)
    else:
        print("No new commits found.")

if __name__ == "__main__":
    check_for_new_commits()
[/code]



<br>===>>update_code_and_restart_nginx.sh </br>
###############################################
#!/bin/bash

# Variables
REPO_URL="https://github.com/username/repository.git"  # Replace with your repository URL
DEST_DIR="/path/to/destination"  # Replace with the destination directory where the code should be cloned
NGINX_SERVICE="nginx"

# Function to clone or update the repository
clone_or_update_repo() {
    if [ -d "$DEST_DIR/.git" ]; then
        echo "Repository already exists. Pulling the latest changes..."
        git -C "$DEST_DIR" pull
    else
        echo "Cloning the repository..."
        git clone "$REPO_URL" "$DEST_DIR"
    fi
}

# Function to restart the Nginx service
restart_nginx() {
    echo "Restarting Nginx service..."
    sudo systemctl restart "$NGINX_SERVICE"
    if [ $? -eq 0 ]; then
        echo "Nginx restarted successfully."
    else
        echo "Failed to restart Nginx." >&2
        exit 1
    fi
}

# Main script execution
clone_or_update_repo
restart_nginx



<br>===>>last_known_commit_sha.txt   </br>
############################################

baa171593dd3dbbd43a29811f5816e6e46b86fe0



