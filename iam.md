# IAM Notes

Knowledge about IAM that I'd frankly not have in my head.

## Granting user access across VMs

```
I was looking through the click detector and realized I needed access to the data. I reached out to Radek, and he suggested using 
radek-fastai-us-west1.

I tried to do so, but it seems I do not have permission. It says, "Ask a project owner to grant you the iam.serviceAccountUser role on the service account."
```

A quick search provides [this explanation](https://cloud.google.com/iam/docs/granting-roles-to-service-accounts). Because the VM is being launched by a service account, we want to grant a user (the identity) the serviceAccountUser role for the service account (the resource).

```
    Open the IAM & Admin page
    Click Select a project, choose a project, and click Open.
    In the left nav, click Service accounts.
    Select a service account, then click Show info panel. All users who can access that service account are displayed.
    Click Add member.
    Add the email address of a project member.
    Select a role for the member to define what actions that member can take against the service account.
    Click Add to apply the role to the project member.
```

Also of note is this bit on [roles](https://cloud.google.com/iam/docs/understanding-roles).

