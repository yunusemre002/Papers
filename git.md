Command line instructions
You can also upload existing files from your computer using the instructions below.


Git global setup  
```
git config --global user.name "Yunus Demir"
git config --global user.email "yunus-emredemir@hotmail.com"
```

Set SSH key 
https://docs.gitlab.com/ee/ssh/index.html#generate-an-ssh-key-pair
```
ssh-keygen -t` 
ssh-keygen -t ed25519 -C "yunus.demir@eliarge.com"
xclip -sel clip < ~/.ssh/id_ed25519.pub
```
sonra git bunu gitlab hesabına ekle.  
```
git clone git@gitlab.com:eliarelektronik/data-science/vtag.git
```


Create a new repository  
```
git clone https://github.com/data-science/...ali.git
cd data_collector_nodejs
touch README.md
git add README.md
git commit -m "add README"
git push -u origin main
```

Push an existing folder  
```
cd existing_folder
git init
git remote add origin https://github.com/data_collector_nodejs.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

Push an existing Git repository  
```
cd existing_repo
git remote rename origin old-origin
git remote add origin https://github.com/data_collector_nodejs.git
git push -u origin --all
git push -u origin --tags
```

# **git tags**
`git checkout master`

### Lightweight tag
`git tag my_lightweight_tag`

### Annotated tag
Açıklamalı, birsürü detay ile birlikte tagleme işlemi yapılmaktadır. Bunu kullanıyorum. Annotated tags are stored as full objects in the Git database. To reiterate, They store extra meta data such as: the tagger name, email, and date. Similar to commits and commit messages Annotated tags have a tagging message.  
`git tag -a v1.0 -m 'Version 1.0'`
* *-a:* annotated 
* *-m:* ile mesaj belirtilir.

### Show list of the existing tags
`git tag`

`git push origin --tags`
