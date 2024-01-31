# Azure_NugetPck_useCases
Use this task to restore, pack, or push NuGet packages, or run a NuGet command. This task supports NuGet.org and authenticated feeds like Azure Artifacts and MyGet. This task also uses NuGet.exe and works with .NET Framework apps.

## Parameters:

The pipeline requires the following parameters to be defined:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| nuSpecFilePathForPackaging | Path to csproj or nuspec file(s) to pack | String | | '**/*.csproj' | Required | Specifies the pattern that the task uses to search for csproj directories to pack. |
| versioningScheme | Automatic package versioning | String | off | off, byPrereleaseNumber, byEnvVar, byBuildNumber | Required |  Allowed values: off, byPrereleaseNumber (Use the date and time), byEnvVar (Use an environment variable), byBuildNumber (Use the build number). Applies automatic package versioning depending on the specified value. This string cannot be used with includeReferencedProjects |
| versionEnvVar | Use an environment variable | String | |  | Required | Specifies the variable name without $, $env, or % |
| configuration | Configuration to package | String | $(BuildConfiguration) |  | Optional | Specifies the configuration to package when using a csproj file |
| packDestination | Package folder | String | $(Build.ArtifactStagingDirectory) |  | Optional | Specifies the folder where the task creates packages. If the value is empty, the task creates packages at the source root |
| includeSymbols | Create symbols package | Boolean | false | true / false | Optional | Specifies that the package contains sources and symbols. When used with a .nuspec file, this creates a regular NuGet package file and the corresponding symbols package |
| toolPackage | Tool Package | Boolean | false | true / false | Optional | Determines if the output files of the project should be in the tool folder |

These parameters provide multiple use case options for the template, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_NugetPck_useCases
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: Azure_NugetPack_useCases.yaml@Template
    parameters:
        nuSpecFilePathForPackaging: ${{ parameters.nuSpecFilePathForPackaging }}
        versioningScheme: ${{ parameters.versioningScheme }}
        versionEnvVar: ${{ parameters.versionEnvVar }}
        configuration: ${{ parameters.configuration }}
        packDestination: ${{ parameters.packDestination }}
        includeSymbols: ${{ parameters.includeSymbols }}
        toolPackage: ${{ parameters.toolPackage }}
        
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

  ```
