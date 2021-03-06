# gcp-ssl-auto-renewer
Auto renew SSL Certificates of Google Cloud Load Balancing with SSL proxy (Cloud LB) in just 1 command.

## Google-managed SSL certificates beta for HTTPS load balancing was released (2018/10). 
You can also use this.

More detail: https://cloud.google.com/load-balancing/docs/ssl-certificates#managed-certs

This project will be depricated.

## Prerequisites

- The domain is managed in Google Cloud DNS.
- GCP Project with Cloud LB enabled
- Install the Let's Encrypt client dehydrated https://github.com/lukas2511/dehydrated
- Install Google Cloud SDK and init in your server

More details in
http://uyamazak.hatenablog.com/entry/2017/07/03/194950


## Install

```
% cd /path/to/install_dir

% git clone https://github.com/uyamazak/gcp-ssl-auto-renewer.git

% cd gcp-ssl-auto-renewer

# install dehydrated
% git clone https://github.com/lukas2511/dehydrated.git

% ls
LICENSE  README.md  config  daily.sh  dehydrated/  example.com.sh  hooks/

# Make your hook file
% cd ./hooks

## Cloud LB
cp httpslb_hook.sh.sample my_httpslb_hook.sh

# Edit variables
% vim my_httpslb_hook.sh

# Make your command file
% cd ../
$ pwd
/path/to/gcp-ssl-auto-renewer

% cp example.com.sh your.example.com.sh
# Edit domain and hook.sh
% vim your.example.com.sh
% chmod 700 your.example.com.sh

# Copy config file to dehydrated's install dir
$ pwd
/path/to/gcp-ssl-auto-renewer
% cp config ./dehydrated

# Check gcloud versions
% gcloud --version
Google Cloud SDK 207.0.0
alpha 2018.06.22
app-engine-python 1.9.71
beta 2018.06.22
bq 2.0.34
core 2018.06.22
datalab 20180503
gcloud
gsutil 4.32
kubectl

# To use dehydrated with this certificate authority you have to agree to their terms of service which you can find here: https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf
# To accept these terms of service run
% ./dehydrated/dehydrated --register --accept-terms
```

## Usage

Run command manually
```
% ./your.example.com.sh
```
Check run messages and ssl certificates from web browser.

if you want to use crontab. Use daily.sh.sample
```
% cp daily.sh.sample daily.sh
% vim daily.sh
```

```
% crontab -e
0 0 * * * /path/to/install_dir/daily.sh 1>>/path/to/log_dir/auto.log 2>>/path/to/log_dir/auto-error.log
```

## About zone name rule
Zone names of Cloud DNS needs to be a domain dots converted to a hyphen.

if domain is: www.example.com

then zone name in Cloud DNS must be: www-example-com

You can change converting rule in httpslb.base.
Edit line below
```
ZONE_NAME=${domain//./-}
```

## Author
uyamazak at bizocean
http://uyamazak.hatenablog.com

