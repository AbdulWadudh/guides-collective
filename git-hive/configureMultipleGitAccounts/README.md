# Guide for Configuring Multiple Git Accounts on Windows OS based on Different Projects/Organizations

## Step-1 : Generating SSH Key
- Open Git Bash and Run the Command with your Config
- I'm Running this in ```C:/Users/{username}/.ssh/``` Folder.
- ``` ssh-keygen -t rsa -C "account1@enmail.com" -f "id_rsa_account1" ```
- The above commant will generate two files
  1. id_rsa_account1
  2. id_rsa_account1.pub
- "id_rsa_account1" can be anything
- Run the same command for Adding Multple Accounts & make sure to assign appropirate identifires for the rsa file

## Step-2 : Adding Genereted SSH Key in Github Account
- We will Add the SSH Key in there Github Accounts
- Goto Github -> User Settings -> SSH and GPG Keys -> New SSH Key -> Give Name as you Like & in Key textarea Paste the Content from **".pub"** file which was generated in **step-1**.

## Step-3 : Registering the New SSH Keys with ssh-agent
- First Run ``` eval "$(ssh-agent -s)" ``` to make sure ssh-agent is running.
- Using ```ssh-add {path-to-another-file-generated-by-ssk-keygen}``` 
- Example ```ssh-add ~/.ssh/id_rsa_account1```
- Run the Same command for other accounts also.

## Step-4 : Creating a SSH Config File for these Accounts
- In the Same folder where our SSH key got generated Create a new file as ```config```.
- Content of the file will as

```
# Account 1, - the default config
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_account1
    
# Account 2
Host github.com-acc-account1    
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_account2
```

- Notice in Account 2 Config we have have added extra keyword to the **Host**.
- So when you want to clone repo which is present in the **account2** 
- you have to add **-acc-account1** to the **github.com**
- Again -acc-account1 can be anything as you like just make sure you use that domain when performing the git commands for that specific accounts

## Step-5 : Creating Git Config files for the Different Accounts
- Will suggest you to create different folder for each accounts
- Inside these folder create a file with ```.gitconfig-account1```
  - Content of these files will be like
    ``` 
    [user]
        name = Account 1
        email = account1@enmail.com
    ```
- Again **-account1** can be anything for the **.gitconfig** file
- Add the Config File to each folder as per how many accounts you want to configure
- Look for the below folder structure for better understanding

```
--Projects_Folder
 --Account_1 //folder
  - .gitconfig-account1 //config file
 --Account_2 
  - .gitconfig-account2
```
![Folder Structure](/git-hive/assets/git-folder-structure-for-multiple-accounts.png)

## Step-6 : Editing the global .gitconfig to segreate the Git User Info
- global **.gitconfig** will be generated in ``` C:/Users/{username}/ ```
- Edit the .gitconfig file and add the fowlling config at the end of the file.

``` 
[includeIf "gitdir:C:/Projects_Folder/Account_1/**"]
    path = C:/Projects_Folder/Account_1/.gitconfig-account1

[includeIf "gitdir:C:/Projects_Folder/Account_2/**"]
    path = C:/Projects_Folder/Account_2/.gitconfig-account2 
```

- So in the above config we are telling the git to use the config of these path when we are working in one of these directories.
- And make sure you add these config at the end of the file.

# Now We have successfully Configured the Multiple accounts for Git. 
### You can test the same by cloning the repo or commiting into the repo. Make sure you are using the domain we have configured for the accounts in Step-4.