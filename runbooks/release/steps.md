The following should be done before releasing a new core mod.

* Set Thunderstore icon
* Set Thunderstore description
* Set Nuget description - this should be done with a <Description> tag in the csproj, and is not in the template by default
* Check dependencies, tags etc in the thunderstore .toml file
* Check that the owner is listed as silksong-modding in:
  - Thunderstore namespace (`silksong_modding`)
  - Thunderstore website (`https://github.com/silksong-modding/REPO`)
  - RepositoryUrl in the csproj (`https://github.com/silksong-modding/REPO`)
  - ID in the BepInAutoPlugin attribute (`org.silksong-modding.*`)
  - Copy target in the csproj (`silksong_modding`)

After checking all of the above:
* Add API keys to the repo
* Set the version to 1.0.0 (or whatever the initial version should be)
* Set the release flag to true in the workflow
  
