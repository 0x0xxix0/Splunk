Follow next doc: https://docs.gitlab.com/ee/install/docker.html


For Linux users, set the path to /srv/gitlab:

```export GITLAB_HOME=/srv/gitlab```

For macOS users, use the user’s $HOME/gitlab directory:

```export GITLAB_HOME=$HOME/gitlab```



docker-compose.yml
```version: '3.6'
services:
  web:
    image: 'gitlab/gitlab-ee:latest'
    restart: always
    hostname: 'gitlab.example.com'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.example.com'
        # Add any other gitlab.rb configuration here, each on its own line
    ports:
      - '80:80'
      - '443:443'
      - '22:22'
    volumes:
      - '$GITLAB_HOME/config:/etc/gitlab'
      - '$GITLAB_HOME/logs:/var/log/gitlab'
      - '$GITLAB_HOME/data:/var/opt/gitlab'
    shm_size: '256m'
 ```
   
Start container 
```docker-compose up -d```   



```docker ps```
```docker exec -it <container ID> sh```

<img width="1257" alt="Screenshot 2023-05-24 at 15 51 21" src="https://github.com/0x0xxix0/GitLub/assets/119075926/24a19cee-89a4-44f9-bb25-3d6a465a7dc3">



```gitlab-rake "gitlab:password:reset[root]"```

<img width="513" alt="Screenshot 2023-05-24 at 15 51 33" src="https://github.com/0x0xxix0/GitLub/assets/119075926/1df9b047-aa27-4cc8-af54-0bcf5d2c3acf">

Credential:

root/P@ssw0rd!!!

<img width="2548" alt="Screenshot 2023-05-24 at 15 52 53" src="https://github.com/0x0xxix0/GitLub/assets/119075926/650a5a01-3903-4dcf-9a96-7a5d2ed499de">
