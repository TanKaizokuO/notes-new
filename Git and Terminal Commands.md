---
tags:
  - main
---
---
# Basic Commands
- pwd : Print current working directory
- ls : list files in current direcctory **(-a for hidden and -l for more details)**
- cd : change directory **(~ : Go home, .. : go up one level, - : go back)**
- mkdir : make directory
- touch : create files
- rm : delete file
-  mv : rename or move file 
	- mv <file_name> <folder_name> : to move 
	- mv <initial_file_name>  <new_file_name> : to rename
- cp : to copy 
	- cp <file_name1> <file_name2> : copy contents of a file 
	- cp -r <folder_name1>  <folder_name2> : copy contents of folder
---

# Git and Github 
- git status 
- git add .
- git commit -m ""
- git clone <url_>
- git pull origin main
- git push origin main

**Git Pull** fetches latest changes on the repo on github and merges that with local repo.
If we have **changes at the same parts of the file on github repo and local repo at the same time** then **merge conflicts** occurs.

We can **ignore local changes and pull the github repo** using 
-  **git stash** : takes the uncommitted changes saves them in a temporary stack which can be view using **git stash list**.
- **git pull** 

If we want to revert back the changes made we use **git stash pop**  which takes the most recent stash and applies iut to the working directory and delete that stash entry if **conflicts arises** during stash pop **changes are applies partially** and the **entry is not deleted**. 

On the otherhand **git stash drop <stash_name >**  deletes the stash entry permanently.

---



