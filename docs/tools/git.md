# Git
Created maandag 07 januari 2013


### disable pop-up password
``# unset SSH_ASKPASS``

### Clone complete set
``# git clone git@github.com:belbob/clw-documentation.git``

### Add some stuff and push to github
``# git add *``

``# git status``

``# git commit -m "added ocs inventory"``

``# git push -u origin master``

### Fetch from github
``# git fetch``

or

``# git pull  (= fetch + merge)``

### working with branch
``# git checkout  -b adminserver  (create branch)``

``# git push origin adminserver  (push to github..or..)``

``# git branch  (show branch)``

``# git checkout adminserver (switch to branch)``

``# git commit -am "added ocs client" (added to adminserver branch)``

``# git checkout master  (switch to master branch)``

``# git merge adminserver  (merge to master branch)``

``# git push  (push to origin master)``


### add ssh-key to github
``# ssh-keygen``

``# cat ~/.ssh/id_rsa.pub``

goto <https://github.com/settings/ssh> and add ssh key

