# Versioning

The following approach is used by the StatTag development team to manage versioning for releases.

## Windows
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

During the development cycle, if we are preparing for the v1.1 release, our first build made available as a beta release would be v1.1.0.0.  If we have another beta build, that would be v1.1.0.1.  If that last beta build is then the release version, it will get incremented one more time to v1.1.0.2.  There is no requirement to have beta release builds - so a patch could just go out as v1.1.1.0.

## Version Schemes
### StatTag Projects
Project | Versioning Scheme
------- | -----------------
StatTag | StatTag Windows versioning scheme
StatTag.R | Copy same version as StatTag
StatTag.SAS | Copy same version as StatTag
StatTag.Stata | Copy same version as Stat Tag

### External Projects
Project | Versioning Scheme
------- | -----------------
DynamicInterop | Increment BUILD for each release containing custom code changes.  Otherwise, leave original version.
RDotNet | Increment BUILD for each release containing custom code changes.  Otherwise, leave original version.
ScintillaNET | Increment BUILD for each release containing custom code changes.  Otherwise, leave original version.

## Tracking Releases
We want to track which versions of each assembly go into a release.  To do that, the main StatTag repository for each platform will have a RELEASE.md file that records each assembly version in a particular build.  This will be tracked for all releases - even beta builds - and should be updated during the release process.

## Release Notes
For now, all bug fixes, enhancements, etc. are tracked on an internal JIRA instance used by the development team, although GitHub will be an important source of bug/request tracking as well.  For each versioned release that is made available to the public, we will have an entry in RELEASE.md that describes the list of changes specific to that build.

For beta releases, this may be a smaller of fixes specific to that build, changed from the last build. When a beta release goes to be an official release, the official release will have the list of changes from the previous release.  This means that if a new feature was not working correctly in between two beta builds, we don't need to mention in the final release notes that we fixed that - we would just mention the new feature.

Because of this and given the nature of how tickets are logged in JIRA and GitHub, the release notes will be a manually curated list.