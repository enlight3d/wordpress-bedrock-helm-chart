# wordpress-bedrock-helm-chart

This chart is for use with the wordpress bedrock. In order to use it you must build a custom php-fpm image with the correspondig directory structure.

The init container of this chart copies the content of /app to /var/www/html. The /var/www/html volume is shared between the nginx and the php-fpm container.

The setup is tested on AWS EKS and sacles well. A typical setup would look like this:

```
Cloudfront (blog.example.com) <-> AWS WF       Cloudfront (static.example.com)
       |                                            |
     AWS ALB                                     S3 bucket (filled by wp offload plugin)
       |
     AWS EKS Cluster
       |
     wordpress pods
     |          |
   nginx      php-fpm
         |
   shared volume (EmptyDir synced from /app)
   
   
         |
      AWS RDS / aurora serverless

```


Features
* seperate containers for nginx (offical image) and php-fpm (custom image)
* seperate pod for cron jobs
* secure default setup (read only volumes, non root containers, hide versions)
* support for exporters and services monitors (see exporters and monitoring section in values.yaml)
* support for wp media offload plugin (see offload section in values.yaml)
* support for external secrets using paramater store (see externalSecrets section in values.yaml)
* support for pre-install, post-install, pre-upgrade and post-upgrade hooks (see hooks section in values.yaml)
* support for hpa (based on cpu, memory, nginx and php connections see hpa section in values.yaml)
* support for custom php config (see php section in values.yaml)
* support for custom nginx config (see nginx section additional_config in values.yaml)
* support for kube-downscaler (see wordpress uptime section in values.yaml)
* support for shared nfs volumes using EFS (see example in values_test.yaml)
* support for custom volumes mounts (see example in values_test.yaml)
* support for custom cron jobs (see example in values_test.yaml)
