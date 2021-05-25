Command line instructions
You can also upload existing files from your computer using the instructions below.


Git global setup  
```
git config --global user.name "Yunus Demir"
git config --global user.email "yunus.demir@eliarge.com"
```

Create a new repository  
```
git clone https://gitlab.com/eliarelektronik/data-science/data_collector_nodejs.git
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
git remote add origin https://gitlab.com/eliarelektronik/data-science/data_collector_nodejs.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

Push an existing Git repository  
```
cd existing_repo
git remote rename origin old-origin
git remote add origin https://gitlab.com/eliarelektronik/data-science/data_collector_nodejs.git
git push -u origin --all
git push -u origin --tags
```