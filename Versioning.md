#Versioning

The following approach is used by the StatTag development team to manage versioning for releases.

##Windows
All code is developed using C#, which has 4 locations for version number parts.  We use a custom mix of [Microsoft's assembly versioning] (https://docs.microsoft.com/en-us/dotnet/framework/app-domains/assembly-versioning) and [Semantic Versioning](http://semver.org).

StatTag version numbers are composed as:

	MAJOR.MINOR.PATCH.BUILD

Where:

Version | Description
--------| -----------
MAJOR   | Releases that have breaking changes and are not backwards compatible OR large feature releases.  We will make every attempt to be backwards compatible, so we anticipate the majority of MAJOR version changes to be for new features or large UI changes.
MINOR   | Interim releases for a group of enhancements and/or bug fixes.
PATCH   | Individual bug fixes that need to go out quickly and can't wait for a MINOR release.
BUILD   | Each time the application is built and released to any group of users (internal or external).  This is manually incremented, and is primarily used for allowing the installer to automatically update new releases of the software, even if it's a new beta release of the same minor release.

Each version number is manually incremented before the build, and should be tagged as a git flow release.

When looking at the StatTag plug-in project, you'll notice that there are multiple projects within the solution.  We typically increment all of the sub-assemblies in a solution to match the main StatTag assembly version.  This applies only to projects created by the StatTag team.  Existing projects (even a custom fork) will just have the BUILD component incremented for each change.

## Version Schemes
Project | Versioning Scheme
------- | -----------------
StatTag | StatTag Windows versioning scheme
StatTag.R | Copy same version as StatTag
StatTag.SAS | Copy same version as StatTag
StatTag.Stata | Copy same version as Stat Tag


## Release Notes