# Secrets

Setting a user secret seems to be as simple as calling init and then setting the secrets. 

Init adds a GUID to the app project file which in turn allows for it to be discovered by the app when it is built, then the build will go and dig it up.

It works like this:

```bash 
dotnet user-secrets init

dotnet user-secrets set "key" "value"
```
