# GitHub Contribution Streak Action

A GitHub Action that automatically updates your profile README with a stylish contribution streak counter.

## 🎯 Features

- Tracks your daily GitHub contribution streak
- Displays a retro-style ASCII art streak counter
- Shows your streak level and rank
- Updates automatically via GitHub Actions

## 🚀 Setup

1. Create a README repository
   - Create a new repository with the same name as your GitHub username
   - This will be your special profile README repository
   - Initialize it with a README.md file

2. Add section markers to your README.md
   Add these markers where you want the streak counter to appear:
   ```
   <!--START_SECTION:streak-->
   <!--END_SECTION:streak-->
   ```

3. Create GitHub Action
   Create `.github/workflows/update-streak.yml` with:
   ```yaml
   name: Update Streak Counter

   on:
     schedule:
       - cron: '0 0 * * *'  # Runs at 00:00 UTC every day
     workflow_dispatch:      # Allows manual trigger

   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v2
         
         - name: Set up Python
           uses: actions/setup-python@v2
           with:
             python-version: '3.x'
             
         - name: Install dependencies
           run: |
             python -m pip install --upgrade pip
             pip install PyGithub requests
             
         - name: Update streak
           run: python update_streak.py
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
             
         - name: Commit changes
           run: |
             git config --local user.email "action@github.com"
             git config --local user.name "GitHub Action"
             git add README.md
             git diff --quiet && git diff --staged --quiet || git commit -m "Update streak counter"
             
         - name: Push changes
           uses: ad-m/github-push-action@master
           with:
             github_token: ${{ secrets.GITHUB_TOKEN }}
             branch: ${{ github.ref }}
   ```

4. Copy the action script
   - Copy `update_streak.py` to your repository

5. Enable Actions
   - Go to your repository's Settings > Actions
   - Ensure Actions are enabled for the repository

## 🎮 Streak Levels

- **ROOKIE**: 0-9 days
- **CODER**: 10-19 days
- **HACKER**: 20-29 days
- **MASTER**: 30+ days

Level increases every 7 days of consistent contributions!

## 📄 License

This project is released under the Unlicense - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Feel free to open issues or submit pull requests.
