# Msbuild Project Dependency Issue

Repro for [Microsoft/msbuild#1915]

In the `*.sln` file, `ProjectSection`s are used to specify build dependencies not originally in the
project files themselves. Visual Studio uses these sections to control the build order. (Project ->
Build Dependencies -> Project Build Order...) Unfortunately, when running `msbuild` from the
command line, this fails with an error. Something like the following:

> ...\Microsoft.Common.CurrentVersion.targets(1601,5): error : Project '...\MsbuildProjectDependencyIssue\MsbuildCoreProject\MsbuildCoreProject.csproj'
> targets 'netcoreapp2.0'. It cannot be referenced by a project that targets '.NETFramework,Version=v4.6.1'. 
> [...\MsbuildProjectDependencyIssue\MsbuildFrameworkProject\MsbuildFrameworkProject.csproj]

## Expected

When running from Visual Studio, either project can be built successfully. Building the
MsbuildFrameworkProject results in the MsbuildCoreProject to get built first. Changes to the
MsbuildCoreProject cause the MsbuildFrameworkProject to get rebuilt.
