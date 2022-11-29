# Docker Project Documentation
I followed this [guide](https://www.hostinger.com/tutorials/run-docker-wordpress) throughout the project.


## Installing Docker and Docker Compose

1. Run the following command to install required packages:
```Bash
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release
```

2. Add the Docker's GPG key.
```Bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3. Set up the repository using the following command:
```Bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

4. Install the following Docker packages and add yourself to the Docker group.
```Bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker areeb
```

5. Install and test Docker Compose. A success message should appear.
```Bash
sudo apt install docker-compose-plugin
sudo docker run hello-world
```
---

## Installing WordPress

1. Create a new directory for **WordPress** and navigate into it.
```Bash
mkdir wordpress
cd wordpress
```

2. Create a **docker-compose.yml** file and insert the contents below:

```Bash
vim docker-compose.yml
i #INSERT MODE
version: "3" 
# Defines which compose version to use
services:
  # Services line define which Docker images to run. In this case, it will be MySQL server and WordPress image.
  db:
    image: mysql:5.7
    # image: mysql:5.7 indicates the MySQL database container image from Docker Hub used in this installation.
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: MyR00tMySQLPa$$5w0rD
      MYSQL_DATABASE: MyWordPressDatabaseName
      MYSQL_USER: MyWordPressUser
      MYSQL_PASSWORD: Pa$$5w0rD
      # Previous four lines define the main variables needed for the MySQL container to work: database, database username, database user password, and the MySQL root password.
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    restart: always
    # Restart line controls the restart mode, meaning if the container stops running for any reason, it will restart the process immediately.
    ports:
      - "8000:80"
      # The previous line defines the port that the WordPress container will use. After successful installation, the full path will look like this: http://localhost:8000
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: MyWordPressUser
      WORDPRESS_DB_PASSWORD: Pa$$5w0rD
      WORDPRESS_DB_NAME: MyWordPressDatabaseName
# Similar to MySQL image variables, the last four lines define the main variables needed for the WordPress container to work properly with the MySQL container.
    volumes:
      ["./:/var/www/html"]
volumes:
  mysql: {}

```

3. Run the following command to start the containers:

```Bash
sudo docker compose up -d
```

4. Open a browser and navigate to the site **http://localhost:8000/**. You will be greeted with a **WordPress** setup page. Fill in all information and log in to access the **WordPress** dashboard.

---

## Screenshot of Dashboard:
![Imgur](https://i.imgur.com/hvHmjXk.png)
