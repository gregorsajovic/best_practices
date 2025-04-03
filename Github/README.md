# Github - how to helper document

## Getting Started with Github

This is short tutorial for working with github

Initialisation of the project's github:

```terminal
git init
```
creates .git/ folder

Creating README.md ile:

```terminal
touch README.md
```

add content to the file and then create remote repository on your github account:

### Create new private repository

##### With git command from terminal:

First set authentication with github token:
1. create token on the github account like described [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

2. add credentials to github global configuration for easy login

```terminal
git config --global user.name YOUR_USER_NAME
git config --global user.email YOUR_EMAIL@ADDRESS
git config --global credential.helper cache
```

When user authenticates for the first time with password/token, then it's not needed to enter password or token again.

To remove credential.helper from global git config use:

```terminal
git config --global --unset credential.helper
```

3. run this command to apply authentication :
   
     ```terminal
     curl -u "USERNAME:GITHUB_TOKEN" https://api.github.com/user
     ```

    To test if permanent connection is established run command:

    ```terminal
      ssh -T git@github.com
    ```

4. to create new repository remotely from terminal, run tihs command:
   
    ```terminal
    curl -u "USERNAME:TOKEN" https://api.github.com/user/repos -d '{"name":"NAME_OF_THE_REPOSITORY", "private":true}'
    ```

   a. to remove repository you can use this curl command:

    ```terminal
    curl -u USERNAME:TOKEN -X "DELETE" https://api.github.com/USERNAME/NAME_OF_THE_REPOSITORY
    or
    curl -L -X DELETE -H "Accept: application/vnd.github+json" -H "Authorization: token TOKEN" -H "X-GitHub-Api-Version: 2022-11-28" https://api.github.com/repos/OWNER/REPO
    or
    curl -L -X "DELETE" -H "Accept: application/vnd.github+json" -H "Authorization: Bearer YOUR-TOKEN" -H "X-GitHub-Api-Version: 2022-11-28" https://api.github.com/repos/OWNER/REPO
    ```

      Of course you have to replace YOUR-TOKEN, OWNER and REPO text with your data accordingly. You can find more info [here](https://docs.github.com/en/enterprise-server@3.10/rest/repos/repos?apiVersion=2022-11-28#delete-a-repository).
   
5. Next step is to add remote origin into settings with this command:
   
```terminal
git remote add origin git@github.com:USER_NAME/NAME_OF_REPOSITORY.git
```

* to check remote branches execute command: 
  ```terminal
  git remote
  ```
* to change remote branch execute command:
  ```terminal
  git remote set-url origin git@github.com:USER_NAME/NAME_OF_REPOSITORY.git
  ```


6. for first push to the github repository execute commands:
```terminal
// adds all files to stack 
git add .
// commits files and adds commit message
git commit -m "initial commit"

// pushes files to the repository
git push --set-upstream origin main
```

* for every next push just use command:
  ```terminal
  git add .
  git commit -m "<YOUR MESSAGE>"
  git push
  ```

7. 


## Working with github - how to

### Adding table of content

H

### Short hints

#### Undo last commit

```terminal
got reset --soft HEAD~
``` 

