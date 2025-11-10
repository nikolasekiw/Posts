# Switching git user per folder on macOS
When I push or merge from Rider IDE, my personal email shows up in the commits on Bitbucket. This isn’t ideal for client projects. The reason this happens is because git is configured globally on my machine with my personal email, since I also use GitHub for personal projects and want my private email associated with those.

To avoid this, I configure a local git identity for each client project folder. This way I can keep my personal and professional git activity separate. Here are the steps for setting up a local git user.

### Step 1. Navigate to the project folder in your terminal:
```bash
cd /path/to/your/project
```

### Step 2. Check the current Git email for this folder:
```bash
git config user.email
```
If nothing is returned, it means the folder is using the global git configuration.

### Step 3. Set a new email for this specific project:
```bash
git config user.email "myemail@myemail.com"
```

### Step 4. Verify the change:
```bash
git config user.email
```
You should now see the updated email address.

### Check your global git configuration
To confirm that your global git user is still set to your personal email:
```bash
git config --global user.email
```
This will show the email configured globally on your machine.
