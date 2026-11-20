# OpusExifTool
All the ExifTool data you can ask for Opus!

`OpusExifTool` is a script add‑in for [Directory Opus](https://www.gpsoft.com.au) that extends the default multimedia columns, letting you pull in any ExifTool value right into Opus. What really sets it apart is its intuitive UI. No more manual edits just to add some columns. Just preview and pick exactly the data you need, then choose how it'll appear in Opus.

> NOTE: Since v3.0, `OpusExifTool` has used its own Go-based helper and `Exiftool.exe`.
> Versions prior to v3.0 require [DOpusScriptingExtensions](https://github.com/PolarGoose/DOpus-Scripting-Extensions). This older versions are considered deprecated.


#### **Key Features:**

* Full support for virtually every property exposed by ExifTool, including unofficial ones.
* Easy to use. Just open the config dialog with a file, or drop a file in, pick the properties you want to add by their preview value, and you're good to go.
* A built-in UI to manage which columns you want to add : customize the name, header, type, category, applicable extensions, and more.
* Designed for both casual and advanced users: the interface is simple but powerful enough to define custom rules and formatting.
* Supports both single-value and multi-stream outputs at runtime, no need to tweak the script manually.
* Real-time preview of values in each property, as they'd appear in the file display.
* Support for [custom columns](#custom-columns) that can take their value from multiple ExifTool fields in a single column.
* Easily switch data types on the fly.
* Import/export your configuration easily.
* Data can be retrieved very fast.
* Support localization for values.
* Built-in support for [caching values](#database) through a SQlite database, based on a configurable file size, with automatic file change detection.
* Dump ExifTool data for desired files through different formats.
* Much more...

<img width="1750" height="913" alt="config" src="https://github.com/user-attachments/assets/e40778ac-a900-4e5d-a6fd-ad311a7fde78" />


---

#### **Requirements:**

* Windows 10 1803 or higher (required for communication with the helper).
* Directory Opus 13.25.4 or higher.
* `ExifTool.exe`.
  - It's already included in the installer. You can also obtain it [here](https://exiftool.org).
  - Alternatively, if you already have it somewhere on your system, you can specify its path in Script Options.

---

#### **Installation:**

This script comes with its own installer, which you can download [here](https://github.com/departure05/OpusExifTool/releases/download/latest/OpusExifToolInstaller.exe).

The installer will put the helper in place and automatically launch the script installation. Optionally, it can also copy `ExifTool` and the related files.

The first time you install the script, you'll be asked if you want to start configuring columns, assuming `ExifTool` is already set up correctly. If it isn't, the script will show you where to download it so you can continue.

<img width="775" height="427" alt="welcome" src="https://github.com/user-attachments/assets/f90de3cb-4f4c-4fa3-9618-688bf28b6cf0" />


Once you've placed `ExifTool` in the appropriate location, you're good to go.

Alternatively, you can open the the `Script Options` window and set the path to your existing exiftool under the `ExifTool && DB` tab.

---

#### **Columns Editor**

<img width="1750" height="913" alt="config" src="https://github.com/user-attachments/assets/e40778ac-a900-4e5d-a6fd-ad311a7fde78" />

To configure which columns are enabled, open the configuration dialog in one of the following ways:

* From the Script Management window, click the Configure button.
* Run the `OpusExifTool CONFIG` command. You can also include the full path to a file you want to preview.

When the dialog opens, if no columns are enabled, you'll be asked to select a file so ExifTool can populate the list with values from its report.

Checked properties will be enabled as columns in Opus once you close the dialog.

ExifTool groups each field into many categories. And each entry can have multiple values.
Use the controls on the right to edit each column's label, header, type, and other settings.

The **Multiple** control lets you choose whether the column displays the property for every stream in its group or only the first one. If **Multiple** is set to `True`, the selected separator will be used to join the values. The list includes a `#` column showing how many values a given keyword has, which is useful when deciding which properties to use.

The **Type** control sets the data type of the column's content. Allowed types are: String (text), number (int), double, star (rating), datetime, date, time, duration, and size.

You can change the type and immediately see how it affects the preview below.

Note: When **Multiple** is set to `True`, the type is always treated as String, since mixing value types in a single column is not recommended.

The **Category** control lets you choose where the column appears. By default, columns are added under Script > OpusExifTool. You can change this to any category you prefer. This doesn't affect the column's behavior or output.

The **Pattern** controls let you apply advanced formatting using regular expressions.

By default, the script uses predefined formats to adjust the type of each value, but you can override them with your own.

The first field defines the pattern to match. Use the same format as an ECMAScript regex literal: `/pattern/flags`.

The second field defines the replacement string. Both follow the format used by JScript's `String.replace(pattern, replace)`, so compatibility depends on that context.

The extensions listed below determine which files the column applies to.

You can add file type groups by entering `grp:` followed by the group's internal or display name.

You can also list specific extensions (including the dot). Wildcards are not supported.

Whenever you open a file for preview, the script saves the listed properties for later use, so you don't have to remember which file had which properties when adding a new column.

You can export or import columns from this window using the provided buttons.

You can also choose which columns should have their contents blurred when using Directory Opus's Secure Screenshot feature.

For keyboard navigation, the following hotkeys are available when the list has focus:

- <kbd>Ctrl</kbd>+<kbd>L</kbd>: Focuses the Label field.
- <kbd>Ctrl</kbd>+<kbd>H</kbd>: Focuses the Header field.
- <kbd>Ctrl</kbd>+<kbd>T</kbd>: Focuses the Type field.
- <kbd>Ctrl</kbd>+<kbd>M</kbd>: Focuses the Multiple field.
- <kbd>Ctrl</kbd>+<kbd>G</kbd>: Focuses the Category field.
- <kbd>Ctrl</kbd>+<kbd>R</kbd>: Focuses the Pattern field.
- <kbd>Ctrl</kbd>+<kbd>A</kbd>: Focuses the Alignment field.

You can also press <kbd>F3</kbd> to move focus to the Filter field.

Press <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd> to toggle the window's always-on-top status (only when opened via the command).

After you close the dialog, the columns will be added automatically and will be ready to use in Opus wherever they apply.

Note: If the columns are already visible, you may need to refresh the lister to see the changes.

---

#### **Options Window**

You can access the Options dialog from here to configure settings such as the script log level, the separator used for multiple values, and options related to [cache usage](#about-cache).

<img width="1057" height="572" alt="options1" src="https://github.com/user-attachments/assets/5472396a-330e-42f1-8c3f-f7dae99e054d" />

<img width="1057" height="572" alt="options2" src="https://github.com/user-attachments/assets/83f08ebd-c609-4c72-8cf4-10a9d72ab370" />

* Assume QuickTime date/time tags are stored as UTC : Assume that integer-valued QuickTime date/time values are stored as UTC, causing conversion to local time when they are extracted. This uses ExifTool's QuickTimeUTC=1 API. More info [here](https://exiftool.org/ExifTool.html#StrictDate:~:text=undef-,QuickTimeUTC,-Flag%20set%20to).

(This dialog is also accessible from the Script Management window by clicking the About button.)

---

### **Custom Columns**

You can create custom columns that pull values from multiple ExifTool fields. You can pick multiple ExifTool fields and order them so the script takes the first value found and shows it as the column value. That way you can have a single column that groups several sources and adapts depending on the file. For example, you can create a column that shows the width of the first video stream for video files, and the image width for image files. There are lots of possibilities.

These columns have the same customization options as the others (type, multiple or not, etc.). The only difference is that their value can depend on multiple fields instead of just one.

To create them, press the "Custom" button in the main UI. That opens a dialog that asks for the name (keyword) of the new column and lets you pick the fields to pull data from and define the processing order.

<img width="829" height="824" alt="custom" src="https://github.com/user-attachments/assets/dd4e235a-7a0c-48ef-97b4-1d165eb907c3" />


You can also edit existing custom columns using the "Edit..." button. (Only appears when a Custom column is selected).

---

### **Database**

This script optionally uses an SQLite database for caching. It includes built-in support for storing data extracted by ExifTool in a database so that subsequent requests can retrieve it quickly and efficiently.

Caching is enabled by default and can be configured in the Options window, accessible from the Column Editor or by clicking the About button in Opus Script Management.

You can also specify the minimum file size for files whose data should be read and stored in the database.

Additionally, you can choose whether caching should be available when Opus is running from a USB export.

You can also define exclusions using wildcards to ignore certain files and prevent them from being added to the database.

The script automatically determines whether a file's data needs to be updated using several metrics, including file size, modification date, and quick hash. This process is transparent to the user.

The script also provides commands for database maintenance and cleanup.

---

### **Cleaning the Database**

You can clean up the database using several command arguments provided by the script.

If you use `PURGEDB` with a wildcard (`*`, `?`) or without a value, the script searches for orphaned database entries, depending on the other arguments used. A list of matching files will be displayed, allowing you to choose which entries to delete and which to keep.

<img width="694" height="659" alt="purge" src="https://github.com/user-attachments/assets/a122e909-ec58-44dc-b221-88cf59e6ed0a" />


If you use it along with `FILES`, you can remove specific entries from the database in a more direct way.


You can also include `VACUUM` to compact the database.


---

#### **Command Arguments**

The command supports the following arguments:

| Argument | Type | Value | Description |
| --- | --- | --- | --- |
| **CONFIG** | /O | | Displays the configuration dialog for columns and infotips. |
| | | *filepath* | Accepts a file path to populate the column preview immediately after startup. |
| **EXPORT** | /S | | Exports columns and infotips to a file. |
| **IMPORT** | /O | | Imports columns and infotips from a file without deleting existing entries. If no file is specified, you'll be prompted to choose one. |
| **!IMPORT** | /O | | Imports columns and infotips from a file, deleting existing entries. If no file is specified, you'll be prompted to choose one. |
| **DUMP** | /M/O | | Saves the ExifTool report for the specified items to a file. You can pass multiple filenames separated by spaces (use commas if a filename contains spaces). If no files are specified, the command uses the selected items in the source folder. |
| **AS** | /K | | Used with `DUMP` to specify the output format. Valid values are `json`, `xml`, `txt`, `csv`, and `html`. |
| **TO** | /K | | Used with `DUMP` to specify the output path. |
| **PURGEDB** | /O | | Performs database cleanup, if applicable. |
| | | *path/wildcard* | If a text value is specified, it can be the full name of a specific file or a path containing `*` and `?` wildcards. If omitted, all entries for non-existing items are removed (`KEEPMISSINGDRIVES` can change this behavior). |
| **KEEPMISSINGDRIVES** | /S | | Used with `PURGEDB` without a specified value to keep entries located on unavailable drives, such as external disks. |
| **VACUUM** | /S | | Compacts the database. Use together with `PURGEDB`. |
| **FILES** | /M | | Used with `PURGEDB` to specifically delete files from the database. |
| **NOONTOP** | /S | | Used with `CONFIG` to prevent the window from opening on top (the default). |

---

#### **Importing from a file**

The command supports importing columns directly from a valid JSON file encoded with UTF-8 NOBOM, with the following format:

<details>

There are one section: **"columns"**.

Each entry (KEY) in both groups must have the following properties:

* **"keyword"**: Cannot be empty.
* **"label"**: Cannot be empty.
* **"header"**:
* **"align"**: Must be `left`, `center`, or `right`.
* **"type"**: Must be `string`, `number`, `number,signed`, `size`, `double`, `duration`, `durationh`, `graph`, `igraph`, `percent`, `datetime`, `date`, or `time`.
* **"category"**: Must be `music`, `movie`, `image`, `script`, `size`, `std`, `date`, or `sums`.
* **"enabled"**: Must be `1` or `0`.
* **"exts"**: Cannot be empty; must include at least one `.` or `grp:`. Multiple values are separated by `; `.
* **"multi"**: Must be `true` or `false`.
* **"regex"**: The regex used for custom formatting.
* **"regex1"**: The regex flags.
* **"regex2"**: The replacement value for the regex.
* **"is_custom"**: Whether this column is a custom one. Must be `true` or `false`.
* **"value"**: Only for custom columns. Cannot be empty.


E.g.
```json
{
    "columns": {
        "Self_WarningCount": {
            "keyword": "Self_WarningCount",
            "label": "Warning Count",
            "header": "Warning Count",
            "align": "right",
            "type": "number",
            "category": "script",
            "enabled": 1,
            "multi": false,
            "use_num": false,
            "regex": "",
            "regex1": "",
            "regex2": "",
            "exts": "grp:Images",
            "is_custom": false
        },
        "Self_ErrorCount": {
            "keyword": "Self_ErrorCount",
            "label": "Error Count",
            "header": "Error Count",
            "align": "right",
            "type": "number",
            "category": "script",
            "enabled": 1,
            "multi": false,
            "use_num": false,
            "regex": "",
            "regex1": "",
            "regex2": "",
            "exts": "grp:Images",
            "is_custom": false
        }
    }
}
```
</details>

---

#### **Notes:**

* ExifTool handles "human-readable" data in addition to conventional values. With this script, you get access to both, and you can choose between them per column, on runtime, by toggling the Use numeric value option.
* The script adds certain values to the "Self" group for convenience: `ErrorCount` and `WarningCount`. Those 2 values are handy in order to find files with corrupted metadata.
* The script adds some common predefined columns with suggested file types. Feel free to edit them, and you can still add more entries to the list whenever you drop a file into it.
* For database caching, the script can automatically detect most file changes and update the stored values using a file-sampling technique that performs micro MD5 checksums. Note that this technique may fail to detect changes in some plain-text files.
* Columns with duplicate labels will be renamed internally to avoid conflicts.
* Custom columns cannot be used in other custom columns or infotips.
* When using `OpusExifTool PURGEDB`, note that checking whether certain paths exist (e.g., slow remote paths) may take longer. During this time, no other operations will be available.
* When the database is enabled, the script includes and populates two additional columns: `Stored` (a Boolean property indicating whether the file has data in the database) and `Last Saved` (the date the file was added to or updated in the database).
* The database stores paths using the volume GUID instead of the drive letter. This is particularly useful for files on removable or USB drives, which are prone to changing drive letters.
* Reports in the database will be automatically updated if the user change language or quicktime date/time related option.
---

#### **Acknowledgments/Credits:**

* ExifTool is free software; you can redistribute it and/or modify it under the same terms as Perl itself (Artistic License 1.0).

    Copyright (c) 2003-2026, Phil Harvey (philharvey66 at gmail.com)

    Official ExifTool website: https://exiftool.org/

* Opus devs, for an amazing program that goes beyond a file manager.

The Go helper uses the following third-party libraries:

* `github.com/goccy/go-json`
* `github.com/mattn/go-sqlite3`
* `golang.org/x/sys`

You can find the latest version and updates [here](https://github.com/departure05/OpusExifTool).

---

