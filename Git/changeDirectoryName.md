Reference from: https://unix.stackexchange.com/questions/424130/change-a-directory-name-in-a-github-repository-remotely-directly-from-local-lin    

The fatal error message indicates you’re working from somewhere that’s not a clone of your git repository. So let’s start by cloning the git repository first:    
```
git clone https://github.com/benqzq/ulcwe.git
```

Then enter it:    
```
cd ulcwe (repo's name)
```
and rename the directory:   
```
git mv local xyz
```
For the change to be shareable, you need to commit it:    
```
git commit -m "Rename local to xyz"
```
Now you can push it to your remote git repository:   
```
git push
```
