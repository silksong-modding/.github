# New Repo Setup

## Purpose

This is a set of steps to follow to set up a new repository for a core mod.

## Prereqs

- Install the [GitHub CLI](https://cli.github.com/)
- Install the latest version of the [modding templates](https://www.nuget.org/packages/Silksong.Modding.Templates) (take the prerelease if applicable)
- Familiarity with basic Git operations

## Steps

1. Check out this repository, which contains some useful template items. 
    `gh repo clone silksong-modding/.github silksong-modding-github`
2. Create the repo, e.g. 
    `gh repo create silksong-modding/Silksong.ModuleName --license EUPL-1.2 --public --clone`
3. `cd` into the repo directory and create the project, e.g.
    `dotnet new silksongplugin --username silksong-modding --thunderstore-username silksong_modding -p`
4. Clean up the template a bit:
    1. Remove the `Silksong_` prefix from the plugin class name and related filename. 
    2. Change the plugin ID from `io.github.silksong-modding.silksong_modulename` to
        `org.silksong-modding.modulename` and remove the todo.
5. Add a license expression to the NuGet package: `<PackageLicenseExpression>EUPL-1.2</PackageLicenseExpression>`
6. Set up csharpier and husky
    1. Add a reference to csharpier in the csproj: 
	    `<PackageReference Include="CSharpier.MsBuild" Version="1.1.2" PrivateAssets="all" />`
	2. Force `lf` line endings for everyone
		1. `echo '* text=auto eol=lf' > .gitattributes`
		2. `echo 'endOfLine: lf' > .csharpierrc.yaml`
    3. ```sh
        dotnet tool install husky
        dotnet husky Install
	    dotnet husky attach Silksong.ModuleName.csproj
	    ```
	4. Copy the default [task-runner.json](./task-runner.json) for husky, e.g. 
	    `cp ../silksong-modding-github/runbooks/task-runner.json .husky/task-runner.json`
    5. Add the husky pre-commit hook:
	    `dotnet husky add pre-commit -c "dotnet husky run --group pre-commit"`
	6. Build the project: `dotnet build`
7. Commit and push the initial project setup.
8. Finish repository configuration.
	1. Open the repository on the web: `gh repo view -w`
	2. Go to the settings tab and change the following settings:
		- Enable release immutability: on
		- Pull requests - allow merge commits: off
		- Allow auto-merge: on
		- Automatically delete head branches: on
	3. Go to the Rules -> Rulesets settings and click "New ruleset." Import [ruleset-no-push-main.json](./ruleset-no-push-main.json)
		from this repo. Scroll to the bottom and create. Unfortunately we cannot do this automatically at the org level
		without paying $5/contributor/month...
