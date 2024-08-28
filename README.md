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
<br>===>>    * * * * * sudo python3 /home/ubuntu/CI-CD-pipeline/script/check_for_new_commits_github_api.py  </br>
<br>===>>   * * * * * /home/ubuntu/CI-CD-pipeline/script/update_code_and_restart_nginx.sh  </br>
<br>===>>  Testing done by updating commit in repo and web url change automatically     </br>


