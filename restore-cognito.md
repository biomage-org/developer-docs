Prerequisites 
For creating and executing backups, you can use the npm package called `cognito-backup-restore` 
(do `npm install cognito-backup-restore` to get the package)

Required IAM permissions

**Downloading user data**

In case there is no cognito user pool or the user pool is empty, we have weekly user backup which executes every sunday at 1am. 
To find the latest users backup file you need to go to biomage-backups-production S3 bucket and find the latest object in the bucket. 
There you can download the `data.json` file, which later will be used to import the users to the new bucket.

In case the users are still existing in a pool you can create a new backup file from the user pool, which can then be used to import users.
Using the package `cognito-backup-restore` you can do `cdr backup`, which is an interactive command and will guide you to choose 
AWS region and user pool name for which you want to create a backup. (more info [here](https://medium.com/geekculture/how-to-quickly-backup-and-restore-aws-cognito-user-pool-c1d820b927a8))

**Restoring Users**
Once you have a .json file with the users you can use the npm package again to restore the users into a cognito pool.
Use the command `cdr restore`. Again, the restore command is an interactive command. It will let you choose the AWS region, 
user pool name and backup file from which you want to restore the users. It will generate a temporary password for the users and send a mail to them with the one time password. 
After login they will be prompted to change their password before proceeding.
