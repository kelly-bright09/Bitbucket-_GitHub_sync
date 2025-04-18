# Bitbucket_GitHub_sync
Mirror and synchronizing GitHub &amp; Bitbucket
# Bitbucket to GitHub Repository Synchronization

![WhatsApp Image 2025-04-16 at 04 35 29_b29e7417](https://github.com/user-attachments/assets/c987d035-ae0c-4a5b-b7fd-186e591556af)


### OBJECTIVE: Migrating your existing bitbucket repo to GitHub repo, synchronizing them in such a way that when ever a change is made in the source repository (Bitbucket) the same change will be replicated in GitHub without any manual intervention.

## Setting up Bitbucket and GitHub
  - On your Bitbucket, create a new repository. 
      Navigate to your Bitbucket repository and Create an access token under Repository settings > Security > Access   tokens.
      Create Repository Access Token with selecting all the "READ" Permissions.    
      Make sure to save it somewhere because you can view it just once.
       Copy the last Access token
    
    ![Image](https://github.com/user-attachments/assets/5b292206-43d0-471c-8f6d-a8c6fa5b2018)

    
      Navigate To Github and Import the Repository while keeping the same name
    
  - On Bitbucket, Enable Pipelines under Repository settings > Pipelines > Settings
    
  ![Image](https://github.com/user-attachments/assets/c71a4852-3257-48f8-8748-991631e4d410)
    
  - On Bitbucket, Generate keys under Repository settings > Pipelines > SSH keys. Copy the public key to clipboard
  - On the same page, under Known hosts enter github.com as the Host address and then click Fetch followed by Add host
  - On GitHub, add the public key under repository settings > Security > Deploy keys > Add deploy key. Tick the checkbox to Allow write access
    
    ![Screen Shot 2024-01-04 at 00 35 39](https://github.com/asaphdanchi/Mirror-and-synchronizing/assets/112729006/2545afe7-52c3-4934-a181-6a1a9b06e447)
    
  - On Bitbucket, add the public key under Repository settings > Security > Access keys > Add key
    
    ![Image](https://github.com/user-attachments/assets/bfe01d08-cce7-4468-91ab-1e7dc17700e4)
  - On Bitbucket, Create an access tokens under Repository settings > Security > Access tokens. Create Repository Access Token with  selecting all the "READ" 
    Permissions and tick the 'Read and write checkbox under Webhooks

    ![WhatsApp Image 2025-04-18 at 01 30 55_de834d30](https://github.com/user-attachments/assets/7d8f0a0c-a82c-4d6c-a8ce-8b10ad330f6f)

  - Copy the first Token
    
  ![Image](https://github.com/user-attachments/assets/135b4e16-ad15-4439-89bf-3e756f88536d)


  - On Bitbucket, Create a Repository variable under Repository Settings > Pipeline > Repository variable. You can name it " BITBUCKET_VARIABLE"
  - The value will be the access token you just created in the previous step  
    
  ![Image](https://github.com/user-attachments/assets/d60b029d-c923-4f96-88ea-d71868e72b69)
  - On Github, At the top right click on your Profile, Scroll down at the bottom click on settings
    

  - At the bottom left of the page click Developer Settings, Create a Personal access tokens Under Personal access tokens > Token (classic) > Generate new token > 
    Generate new token (classic)
    
   ![Image](https://github.com/user-attachments/assets/34a6e55b-bcc9-4c92-8b27-78ed257d8b4e)
    
  - Check the "repo" box, "Workflow" and the "write:package" box.
   
  ![Image](https://github.com/user-attachments/assets/d19a51cb-beee-4d07-90a9-585854f75e0e)
  -  Scroll down and click Genarate Token and copy the token

  - On Bitbucket, Create a Repository variables with the Personal Access Token from Github as the value. Repository Settings > Pipeline > Repository variable. You 
    can name it "GITHUB_VARIABLE".

   ![Image](https://github.com/user-attachments/assets/b0253da0-8f4f-4ae9-8d45-7e414255d6b5)

## Creating a bitbucket-pipelines.yml file

![Image](https://github.com/user-attachments/assets/0284fe56-d914-46f1-98e3-000e1daa3588)

Paste the following:

```
  pipelines:
    default:
      - step:
          name: Sync GitHub Mirror
          image: alpine/git:latest
          clone:
            enabled: false
          script:
            - git clone --mirror https://x-token-auth:"$BITBUCKET_VARIABLE"@bitbucket.org/solavisetech_internship/bitbucket-github_sync.git ## @bitbucket.org follow by your Bitbucket repository path
            - cd bitbucket-gitHub_sync.git ## cd followed by your Github repository Name
            - git push --mirror https://x-token-auth:"$GITHUB_VARIABLE"@github.com/asaphdanchi/Mirroring-Repo.git ## @github.com followed by your Github repository path
```

On your code replace the "$BITBUCKET_VARIABLE" and "$GITHUB_VARIABLE" with your corresponding variable names while keeping the   $ and the "" sign. 

Bitbucket repository path: ![Image](https://github.com/user-attachments/assets/facaab48-8362-4dac-91f3-a4431fba43fe)
GitHub repository path: ![Image](https://github.com/user-attachments/assets/4242e71c-0250-4e8c-b5de-081bbc14a481)
GitHub repository name: ![Image](https://github.com/user-attachments/assets/a209f5d2-5b37-459e-bf63-460604448628)

![Image](https://github.com/user-attachments/assets/aaf4168f-1b4f-4527-bd66-d6eec1869a6d)

## Running the pipeline in Bitbucket


![Image](https://github.com/user-attachments/assets/23fff02f-6291-413a-a1a2-6ad3cc3cfc00)
## Now check your GitHub repository and see that the bitbucket-pipelines.yml was automatically added!

![image](https://github.com/user-attachments/assets/8a498d1b-01c8-4c7e-8594-db7937ccdfd1)
