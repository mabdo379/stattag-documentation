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