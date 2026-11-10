# New Repo Setup

## Purpose

This is a set of steps to follow to set up a new repository for a core mod.

## Prereqs

- Install the latest version of the [modding templates](https://git.silksong-modding.org/silksong-modding/-/packages/nuget/silksong.modding.templates) (take the prerelease if applicable)
- Familiarity with basic Git operations

## Steps

1. Go to https://git.silksong-modding.org/silksong-modding/repo-template and click "Use this template".
   1. Name the repo `Silksong.<ModuleName>`
   2. Check "Git content" and "Branch protection"
2. Clone the created repo locally and create a branch for the changes you'll make.
3. `cd` into the repo directory and create the project, e.g.
    `dotnet new silksongplugin --username silksong-modding --thunderstore-username silksong_modding -p`
4. Adapt the repo to Forgejo
   1. Move the contents of `.github` into `.forgejo`
   2. Delete `dependabot.yml`
5. Add a license expression to the NuGet package: `<PackageLicenseExpression>EUPL-1.2</PackageLicenseExpression>`
6. Set up csharpier and husky
    1. Add a reference to csharpier in the csproj: 
	    `<PackageReference Include="CSharpier.MsBuild" Version="1.1.2" PrivateAssets="all" />`
    2. ```sh
        dotnet tool install husky
        dotnet husky Install
	    dotnet husky attach Silksong.ModuleName.csproj
	    ```
    4. Add the husky pre-commit hook:
	    `dotnet husky add pre-commit -c "dotnet husky run --group pre-commit"`
	5. Build the project: `dotnet build`
7.  Commit and push the initial project setup, and submit a PR for the change.
8.  Set up push-mirroring in GitHub.
    1.  Create a new repository with the same name in GitHub.
    2.  Go to the Rulesets tab in the repo settings. Import the 2 rulesets included in this directory to block human pushes to branches and tags.
    3.  From the repo homepage, copy the *SSH* address for the repo. It should look like `git@github.com:silksong-modding/RepoName.git`
    4.  In Forgejo, open the repo settings and scroll down to Mirror Settings
        1. Git remote repository URL: Pasted from step NNNNNN
        2. Authorization: Leave username and password blank and tick "Use SSH authentication"
        3. Tick "Sync when commits are pushed"
     5. Click "Add push mirror" and then scroll down to copy the SSH key
     6. Go to the repo settings in GitHub
        1. Scroll down to the "Features" section and untick everything
        2. Go to "Deploy Keys" and add a deploy key
           1. Set the name as "Forgejo push mirror"
           2. Paste the key obtained from Forgejo
           3. Tick "Allow write access"
           4. Add the key
     7. Go back to Forgejo and click the refresh button on the push mirror. Wait a few seconds and refresh the page to confirm the push succeeded with no issues.
