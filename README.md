# jenkins-update-center  

Jenkins mirror update center generator


## Steps to create local mirror of Jenkins update center:

1. Clone code:

```bash
git clone https://github.com/JantsoP/jenkins-update-center.git 
```

2. Generate self-signed certificate:

```bash
cd jenkins-update-center
mkdir rootCA
openssl genrsa -out rootCA/update-center.key 2048
openssl req -new -x509 -days 3650 -key rootCA/update-center.key -out rootCA/update-center.crt
```

3. Rsync into your www root directory:

```bash
rsync -avz --delete rsync://rsync.osuosl.org/jenkins/ /var/www/jenkins
```

4. Install dependencies:

```bash
yum -y install make gcc automake autoconf python3-devel git
pip install -r requirements.txt
```
5. Setup mirrors json:

   > Set your mirror url
   
   ```
   # mirrors.json
   {
       "localhost": "http://localhost/jenkins/"
   }
   ```
   
6. Run the following python code:

```bash
python3 generator.py
cp localhost/update-center.json /var/www/jenkins/
```

1. Put `update-center.crt` into `${JENKINS_HOME}/update-center-rootCAs` folder. chown it to jenkins:jenkins. Use chmod 640 on the file.
2. Go to `Jenkins → Manage Jenkins → Manage Plugins → Advanced → Update Site` and submit URL to your `update-center.json`.

