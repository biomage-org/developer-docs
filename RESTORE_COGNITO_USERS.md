# 1. Prerequisites 
For creating and executing backups, you can use the npm package called `cognito-backup-restore`.
To install:
`npm install -g cognito-backup-restore`

### Required IAM permissions
To be able to do the user restoration you need to be in the `engineering` group in AWS. If you are a developer you should already have those permissions.  If you need to be added to the permission group, speak with Iva or Pol, who will add it through IAM.

# 2. Downloading user data
Here do either 2.1 or 2.2 depending on the situation.

### 2.1. In case there is no cognito user pool or the user pool is empty 
We have weekly user backup.  You will need to find the latest users backup file. To do that go to `biomage-backups-production-{accountId}` S3 bucket and find the latest object in the bucket. The objects names are the date they are created so using this you can find the latest one.
After that download the `data.json` file inside that object. Later it will be used to import the users to the new/existing user pool.

### 2.2. In case the users still exist in a pool
Create a new backup file from the user pool, which can then be used to import users.
Using the package `cognito-backup-restore` you can do `cbr backup`, which is an interactive command and will guide you to choose 
AWS region and user pool name for which you want to create a backup. The command lists all the available options so it will be easy for you to choose the correct parameters. Use the arrow keys and enter for choosing. (more info [here](https://medium.com/geekculture/how-to-quickly-backup-and-restore-aws-cognito-user-pool-c1d820b927a8))

# 3. Create new user pool if required (if the old one does not exist)

If the user pool does not exist anymore you should create a new one. The user pool cloudformation is defined in the [iac](https://github.com/hms-dbmi-cellenics/iac) repository in the file called
`cognito-case-insensitive.yaml`. You can trigger Cloudformation to create the pool again by opening a new PR in the repository with the file modified
(add a new line to the file). After merging, the stack will be synchronised and the user pool should be created again.

# 4. Restoring Users
Once you have a `.json` file with the users and an existing user pool you can use `cognito-backup-restore` again to restore the users into the cognito pool.
Use the command `cbr restore`. Again, the restore command is an interactive command. It will let you choose the AWS region, 
user pool name and backup file from which you want to restore the users. It will generate a temporary password for the users and send an email to them with the one time password. 
After login they will be prompted to change their password before proceeding.
