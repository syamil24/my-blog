---
title: I Reduced my .git Folder From 2GB to Just 16 MB!
created: '2025-08-03T06:51:36.809Z'
modified: '2025-08-03T16:17:06.115Z'
categories: [Programming, IT, coding, Git, devops]
tags: [coding, IT, devops]
date: '2025-08-03'
---

![Desktop View](/assets/git-size-reduce/git3.png)

It's been more than one year when I started working on this project. The 2GB .git folder size don't really bother me since it just a one time clone, and the rest of the flow (fetch, commit, pull, etc) does not really taking my bandwidth 2GB all time. And then my concern came in when my jenkins pipeline that just working fine before and now it keep failing. Tried viewing at the logs, and I saw that the git plugin is reaching 10 minutes timeout executing this command: `git fetch --tags --force --progress -- https://github.com/MY_GIT_REPO_HERE +refs/heads/*:refs/remotes/origin/* # timeout=10`. Although it just the same 2GB git folder like before, another issue occured in this Jenkins server is where the network speed lately is much slower than before, on average speed less than 1Mbps.

Since nothing much I can do on the network level (yep this is my company server controlled by the Infra team), the choices I left only on trying to reduce the git size so that with slower network speed it will still managed to execute the command without reaching the timeout.

P/s: If you are using Windows, you need to use Git Bash terminal as some of the commands does not work properly in CMD or Windows Terminal. Yes make sure to run this in admin as well

**### Finding the root cause... Windows Dump File ???**

And here is where the phase where searching online got me an idea that the reason why .git folder size is too big is due to somebody has commit unnecessary large files in your codebase. Searching online gave me few commands related on this but none of it is the exact of what I want. And here ChatGPT comes to help. Asking some prompt, providing some context and ChatGPT gave me this where it will automatically sort top 10 largest objects in my commit history:

```cmd
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | sort -k3 -n -r | head -n 10
```

BAM!! All the top 10 largest files in my commit history listed out here. And guess what, back few years when this project was kick started some random developer just push .dmp files in this repo. WTH?? How come you are copying this dump file in your repo directory and somehow committed it? Bruh....

![Desktop View](/assets/git-size-reduce/git2.png)

**### git-filter-repo comes to the rescue**

So how do I get rid of these unncessary commits in my repo? This is where git-filter-repo useful where it will rewrite git repository history and remove these related commits. According to their github repo, filter-repo requires:

- *git >= 2.36.0*
- *python3 >= 3.6*

**REMINDER: PLEASE BACKUP YOUR PROJECT FIRST, AS THIS EXECUTION WILL BE DESTRUCTIVE AND REWRITE YOUR GIT HISTORY. IT ALSO WILL REMOVE REPO ORIGIN CONFIG FROM YOUR PROJECT**

To install it, run this:

```bash
pip install git-filter-repo # (need to run as admin)
```

Once installed, this is the most important one where you will delete all commit history that contains the large file and rewrite back the commit tree.
Reminder: this command is **destructive**! since it will rewrite your commit tree and will remove the remote origin url you have configured  in the project. So make sure you have a backup to prepare for worst case scenario.

```cmd
git filter-repo --force --path-glob *.dmp --invert-paths
```

Then, since your origin url is removed, add back your remote origin url. And now time to push it back to your remote origin and rewrite the commit tree in the remote. To be safe, checkout to another branch first before commit directly to your main/master branch.

```cmd
git remote add origin https://YOUR_REMOTE_URL
git push origin --force --all
git push origin --force --tags
```

And that's it. I managed to reduced my .git folder from 2gb to just 16mb as screenshot above due to unnecessary Windows Dumps file committed back few years ago.

### More ways to explore

**git-filter-repo** offering much more features in terms of rewriting git commit history than what I've shown here. Check out it repo here to learn more:
<https://github.com/newren/git-filter-repo>

You can find all the commands above in my gist here, just copy and paste to your project
<https://gist.github.com/syamil24/fe79aa1d54ebf8ae91cc487f668e10cb>

And there could more other ways to explore to achieve the same objective. Perhaps you can find a better one than this!

