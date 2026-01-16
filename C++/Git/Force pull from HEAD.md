```sh
# 1. Fetch to see what's on remote
git fetch origin

# 2. Check what you're about to lose
git diff origin/branch-name

# 3. Force your local to match remote
git reset --hard origin/branch-name
```