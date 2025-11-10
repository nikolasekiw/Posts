# Fixing node version issues 
> A quick guide to resolving common Node.js version errors when building a project

When running `npm build`, you might encounter an error like this:

```
Error: error:0308010C: digital envelope routines::unsupported
```

This often indicates a mismatch between your Node.js version and the requirements of the project you're working on. Here's how you can troubleshoot and fix it.

### Step 1: Check Your Current Versions
Start by checking which versions of Node.js, npm, and nvm you have installed:
```bash
$ nvm -v        # → 0.38.0
$ npm -v        # → 10.2.4
$ node -v       # → v18.19.1
```

### Step 2: List and Manage Node Versions
Use `nvm` (Node Version Manager) to list and switch between Node.js versions:
```bash
$ nvm list              # Lists all installed Node.js versions
$ nvm install 16.14.0   # Installs the required version
$ nvm use 16.14.0       # Switches to the installed version
```

### Step 3: Clean Up and Reinstall Dependencies
After switching Node versions, it's a good idea to remove existing `node_modules` and reinstall dependencies to avoid conflicts:
```bash
$ rm -rf node_modules   # Removes all installed packages
$ npm install           # Reinstalls dependencies
$ npm run develop       # Starts the development server
```

### Summary
If you're getting cryptic errors during `npm build`, it can be due to an incompatible Node.js version. Using `nvm` to switch to the correct version and reinstalling dependencies often resolves the issue.
