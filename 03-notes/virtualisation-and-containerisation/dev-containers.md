# Dev Containers

## Current

Currently I have this working quite nicely using a pretty rudimentry dev container set up through vs code, nb this is only really local set up 

I also have a pretty decent grasp on the different ways to use Azure Bastion to connect to these things. Notes on that:

- You ssh into the machine using a tunnel first, then effectively ssh'ing to the local host.
- I assume that I will be able to use docker contexts to effectively set the build machine as the docker host, via a bastion tunnel

## To Explore

Things to look at from here, would be how to get the local set up onto the remote server in a useful way. Currently thinking that I will install docker on the remote machine, spin up a bastion tunnel and use docker context to connect to it.

See: `https://medium.com/@rajaravivarman/extending-docker-using-docker-context-to-manage-remote-containers-66b8abc5d245`

Docker on WSL Just in case: `https://www.reddit.com/r/docker/comments/1d7r2qi/guide_how_to_run_docker_on_a_windows_host_without`

Use SSH-Key Gen to create a private key

SSH Keys Quick Guide: `https://code.visualstudio.com/docs/remote/troubleshooting#_container-tips`

Then follow the instructions that are on the ms docs: `https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement`

` ssh pd57215@127.0.0.1 -p 50022` should work

```
# By default, the ssh-agent service is disabled. Configure it to start automatically.
# Run the following command as an administrator.
Get-Service ssh-agent | Set-Service -StartupType Automatic

# Start the service.
Start-Service ssh-agent

# The following command should return a status of Running.
Get-Service ssh-agent

# Load your key files into ssh-agent.
ssh-add $env:USERPROFILE\.ssh\id_ecdsa
```

```
# Get the public key file generated previously on your client.
$authorizedKey = Get-Content -Path $env:USERPROFILE\.ssh\id_ecdsa.pub

# Generate the PowerShell command to run remotely that copies the public key file generated previously on your client to the authorized_keys file on your server.
$remotePowershell = "powershell Add-Content -Force -Path $env:ProgramData\ssh\administrators_authorized_keys -Value '''$authorizedKey''';icacls.exe ""$env:ProgramData\ssh\administrators_authorized_keys"" /inheritance:r /grant ""Administrators:F"" /grant ""SYSTEM:F"""

# Connect to your server and run the PowerShell command by using the $remotePowerShell variable.
ssh username@domain1@contoso.com $remotePowershell
```

And then for actually getting this to look like a thing that might be good and useful, we miiiiight be able to use the instructions in the 'use existing devcontainers.json' part of this document:`https://code.visualstudio.com/remote/advancedcontainers/develop-remote-host`

### Workspaces

Seems to me that in and around the easiest way to manage most of this stuff is through Workspaces. Obvs, if you could just attach cloned containers as volumes, that would be even better. But there are restrictions on that which is real annoying.

Also workspaces are saved as actual files? I did not know that and am honestly not sure how I feel about it.

## Source Code Management

I think the best way to manage this, is to create a dev container using the c# image, then attaching the source code using a volume mount

If you are going to do that, you need to share your SSH keys (from your host) to your container

Easiest way to do this is run:

`ssh-keygen -t rsa` then hit enter three times
`ssh-add` 
Then find the key you have generated and paste that into DevOps

Refer here for more details: `https://code.visualstudio.com/remote/advancedcontainers/sharing-git-credentials`

You then need to actually authenticate to DevOps for some reason. Open termianl instance and paste the following: `ssh -T git@ssh.dev.azure.com`

After all of that, you should be able to clone into a named vol. If not, refer docs: `https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection`

I think you might have to run this every day

## Questions and Thoughts

### Answers to the below

1. I think you can do it in either order, but I think it makes the most sense to go clone into vol first, which will then prompt you to go through the process of attaching to a dev container

### Questions

1. Do I create the Dev Container first then attach the cloned vol? Or do I clone the vol and then create a dev container to attach it to?
    1.1 Indirectly I think one of the blokes on Reddit has suggested clone vol first then open that vol in a dev container
    1.2 I am however trying it the other way around at the moment, if for no other reason than I would like to get a W on  the board

## What to Install in Dev Containers

I think it is probably a given at this point that we will be running the c# / .net containers, but if we run the 9.0 sdk will that cover us for the lower versions? Probably not, is it annoying to switch versions for that reason only?

Extensions obvs

## Container Volumes

I think it is better to go container volume -> container, but then possibly you might run into the issue with git clone not being successful, because the git credentials have not yet been shared with the container (ssh keys)

If I had a new machine I would test this out.
