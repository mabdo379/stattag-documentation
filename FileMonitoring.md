# File Monitoring
On both Windows and macOS, we are able to detect the following types of file change events:

* Contents Changed
* Renamed or Moved
* Deleted

When monitoring files, the following activities are processed in order of precedence, which allow us to alert the user in a simplified, summary form:

1. If a file was deleted, we will tell the user that and stop there. No other actions really matter after that.
2. If a single file moved:
  1. No matter how many times the move takes place, we only care about the _last_ time. (origin to target)
  2. If that last time puts it back into the same location (as the origin path), let's not bother the user
3. If a file's contents were modified, let's tell the user that the content changed.  We are okay being a little naive here and just blindly detecting the file was updated.
   
For each code file we're going to wind up with one of two possible outcomes:

1. A delete
2. A combination of a single file change and/or single file move

## File Monitor Management
When a code file is linked to a Word document, StatTag will add a file monitor.  When an existing Word document is opened, StatTag will add a file monitor for all existing code files that exist.  If a referenced code file does not exist, StatTag will not add a monitor.  Assuming the user manually connects the missing code file, StatTag will then activate a file monitor for the file.

When StatTag is preparing to write to a code file, it will temporarily stop the file monitoring process.  Otherwise we will alert the user that the code file changed when it was us who made the change.

## macOS Implementation
Within the macOS version, because StatTag is implemented as a standalone application, we queue up the list of file changes behind the scenes.  Once the StatTag application becomes active, we check with all of the file monitors to see what relevant actions took place.  Per earlier comments, we may only return a single delete event (since that would block any other changes).  For others, we will reconcile from the full list of changes what the end state is of the code file and alert the user.

## Windows Implementation
Within the Windows version, because StatTag is implemented as an add-in within Word, we monitor and manage alerting for code file changes based on the active document.  The file monitors will queue all changes that take place.  As these events take place, we will have StatTag check to see which document is currently active.  For that active document, we will set a timer to alert the user of the change.

## Vignettes
### Scenario 1 - Editing Code Externally
**_Workflow_**: Susan is in Microsoft Word on Windows, and links her current Word document to an R code file located at `C:\Stats\test.r`.  She creates a few tags within the StatTag plug-in, but realizes as she begins inserting results that she forgot a part of her analysis.  She has confirmed this by bringing up the "Manage Tags" dialog, and has the tag (code) editor open for the last tag she added.  She opens up RStudio and begins making changes to the code in `C:\Stats\test.r`.  After testing that her code runs from RStudio, she brings her Microsoft Word document back up.

**_Expected Behavior_**: Every time that Susan clicked 'Save' in RStudio, StatTag received a notification that `C:\Stats\test.r` had been modified.  It detected that the Word document wasn't actively in the foreground (it was minimized because she was using RStudio), and so StatTag kept setting a timer to check again later for when the document was active.  When Susan brought her Word document back to the foreground, the timer elapsed and StatTag was able to see the document was in the foreground.  Realizing that the underlying code file was changed, StatTag closes (without saving) the tag editor window, closes the "Manage Tags" dialog, reloads `C:\Stats\test.r` within StatTag, and provides a message box alerting Susan that the code file was modified and has been refreshed within StatTag.