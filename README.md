# Poetry GHE Dependency Sample
We are using self hosted Github Enterprise Server. So, we have a customized url for github enterprise, which is https://github-partner.azc.ext.com. 
In this sample, you can see that, I have a git repo dependency in pyproject.toml file. This repository is in our github enterprise server instance.
Along with that, I have a python dependency Jinja2

For the dependent GHE repo 'platform-tools', a new version 'v0.6.0' is released and tagged

## Expected behavior
From the renovate release notes for version 35.70.0, we know that poetry should support tag as git-dependency. [PR#21949](https://github.com/renovatebot/renovate/issues/21949) implemented this feature.
When Renovate runs, I am expecting two pull requests, one to update Jinja2 and another one to update 'platform-tools' to latest tag v0.6.0.

## Current behavior 
Jinja2 is being updated and pull request is created. But the git dependency 'platform-tools' is not updated and no pull request is created.

## What we have found.
I have checked the Renovate source code looking for a reason. In [schema.ts file line#45](https://github.com/renovatebot/renovate/blob/f7a381e7b17e09aa6db818e4af81787ffc96f7db/lib/modules/manager/poetry/schema.ts#L45) I found that, the code checks if the dependency url is 'github.com'. But since we are using self hosted Github Enterprise Server, we have custom url 'github-partner.azc.ext.com'. I believe this is the reason for this issue. I do not understand why does the code check the url of the git dependency. In the same file, the code does not check the url if 'rev' is used in the toml file instead of 'tag'
