# OpusExifTool
All the ExifTool data you can ask for Opus!

`OpusExifTool` is a script add‑in for [Directory Opus](https://www.gpsoft.com.au) that extends the default multimedia columns, letting you pull in any ExifTool value right into Opus. What really sets it apart is its intuitive UI. No more manual edits just to add some columns. Just preview and pick exactly the data you need, then choose how it'll appear in Opus.

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
* Support for regular ExifTool CLI.
* Support localization for values.
* Built-in support for [caching values](#about-cache) based on a configurable file size, with automatic file change detection.
* Dump ExifTool data for desired files through different formats.
* Much more...

<img width="1448" height="850" alt="column_editor" src="https://github.com/user-attachments/assets/ac5e67dc-381b-48aa-8b38-c2b75d1b4d69" />

---

#### **Installation:**

This script fully support `DOpus-Scripting-Extensions` by @PolarGoose. First, you'll need to install the latest version from [here](https://github.com/PolarGoose/DOpus-Scripting-Extensions/releases).
Additionally, you can also use the regular ExifTool.exe CLI (including localization files), and put it in `/dopusdata\User Data\OpusExifTool`.

Then install the script as usual. (Required at least DOpus v13.21.1).

<a name="release">Latest version</a> : [Download from here](https://github.com/departure05/OpusExifTool/releases/download/latest/OpusExifTool.opusscriptinstall)

You can always find the latest version published here.

---

#### **Columns Editor**

The first time you install the script, it'll ask if you want to start configuring columns, assuming you already have ExifTool set up correctly. If not, it'll give you some options to continue.

<img width="646" height="267" alt="welcome" src="https://github.com/user-attachments/assets/249509a9-332b-49f7-b0f4-5737546f13b5" />

To configure which columns are enabled, you need to open the configuration dialog, which you can do in a few different ways:

* From the Script Management window, click the Configure button.
* Using the `OpusExifTool CONFIG` command. You can also include the fullpath to a file you'd like to preview.

When this dialog opens, if there's no columns enabled, you'll be asked to select a file so ExifTool can fill in the list with values from its inform.

<img width="1448" height="850" alt="no_columns" src="https://github.com/user-attachments/assets/f8453813-8d10-498e-b195-04def13cd501" />

<img width="1448" height="850" alt="column_editor" src="https://github.com/user-attachments/assets/ac5e67dc-381b-48aa-8b38-c2b75d1b4d69" />

Checked properties are the ones that will be enabled as columns in Opus once you close the dialog.

ExifTool groups each field into many categories. And each entry can have multiple values.
Using the controls on the right, you can edit each column's label, header, type and so on.

The "Multiple" control lets you choose whether the column will show the property for each stream in the group, or just the first one. If "Multiple" is set to "True", the chosen separator will be used to split the values. The list has the "#" column, which shows how many values a given keyword has, useful for deciding its value.

The "Type" control sets the data type of the column's content. Allowed types are: String (text), number (int), double, star (rating), datetime, date, time, duration, and size.
You can change this value and immediately see how it affects the preview below.
Note: When "Multiple" is **True**, the type is always treated as String, since it's best practice not to mix value types in a single column.

The "Category" control lets you change where the column appears. By default, columns are added under Script > OpusExifTool. You can switch this to any other category you prefer. This doesn't affect the behavior or output of the column.

The "Pattern" controls let you apply advanced formatting using regular expressions.
By default, the script uses predefined formats to adjust the type of each value. You can override those with your own.
The first field is where you define the actual pattern to match. Use the same format as ECMAScript literal regex: `/pattern/flags`.
The second field defines the replacement string. Both follow the same format used in JScript's `String.replace(pattern, replace)`, so compatibility depends on that context.

The listed extensions below are used to determine which files the column applies to.
You can add file type groups using `grp:` followed by the internal or display name of the group.
You can also list specific extensions (include the dot). Wildcards are not supported.
ExifTool provides for some fields a numeric value along with the human-readable one. You can choose which one to choose on a per-field basis by toggling the "Use the numeric value" checkbox.

Every time you open a file for preview, the script saves the listed properties for later use, so you don't have to remember which file had which properties when adding a new column.

You can Export or Import columns from this same window, using the provided buttons.

<img width="644" height="221" alt="import_export" src="https://github.com/user-attachments/assets/d8b3c284-32cf-4d86-b8b1-c0eea39e1f24" />

For keyboard navigation, the following hotkeys are active when the list is focused:
<kbd>Ctrl</kbd>+<kbd>L</kbd>: Focuses the Label field
<kbd>Ctrl</kbd>+<kbd>H</kbd>: Focuses the Header field
<kbd>Ctrl</kbd>+<kbd>T</kbd>: Focuses the Type field
<kbd>Ctrl</kbd>+<kbd>M</kbd>: Focuses the Multiple field
<kbd>Ctrl</kbd>+<kbd>G</kbd>: Focuses the Category field
<kbd>Ctrl</kbd>+<kbd>R</kbd>: Focuses the Pattern field
<kbd>Ctrl</kbd>+<kbd>A</kbd>: Focuses the Alignment field

You can also press <kbd>F3</kbd> to move focus to the Filter field.

After closing the dialog, the columns will be added automatically and will be ready to use in Opus, in any field where they apply.
Note: If the columns are already visible, you may need to refresh the lister to see the changes.


You can also access the Options dialog from here, which lets you configure things like the script log level, the separator used for multiple values, and settings related to [cache usage](#about-cache).

<img width="802" height="417" alt="options" src="https://github.com/user-attachments/assets/10efac77-9ab1-4931-a3e7-05e3adb8941c" />

(This dialog is also accesible from the Script Management window, by clicking the About button).

---

### **Custom Columns**

You can create custom columns that pull values from multiple ExifTool fields. You can pick multiple ExifTool fields and order them so the script takes the first value found and shows it as the column value. That way you can have a single column that groups several sources and adapts depending on the file. For example, you can create a column that shows the width of the first video stream for video files, and the image width for image files. There are lots of possibilities.

These columns have the same customization options as the others (type, multiple or not, etc.). The only difference is that their value can depend on multiple fields instead of just one.

To create them, press the "Custom" button in the main UI. That opens a dialog that asks for the name (keyword) of the new column and lets you pick the fields to pull data from and define the processing order.

<img width="1448" height="850" alt="custom_column" src="https://github.com/user-attachments/assets/e54103d7-2b5c-4e63-9ff9-cabc62f3c67c" />

You can also edit existing custom columns using the "Edit..." button. (Only appears when a Custom column is selected).

---

#### **Command's Arguments**

The command support the following arguments:

| Argument | Type | Value | Desc |
|----|----|----|----|
| **CONFIG** | /O |  | Shows the dialog windows for configuring columns. |
|  |  | *filepath* | Accepts a filepath to populate the preview column right after start. |
| **EXPORT** | /S |  | Exports columns to a file. |
| **IMPORT** | /O |  | Imports columns from a file without deleting existing ones. If no file is specified, you'll be prompted to choose one. |
| **!IMPORT** | /O |  | Imports columns from a file, deleting existing ones. If no file is specified, you'll be prompted to choose one. |
| **DUMP** | /M/O |  | Save the ExifTool report for the chosen items to a file. You can pass multiple filenames separated by spaces (use commas if a filename contains a space). If no files are passed, the command will use the selected items in the source folder.|
| **AS** | /K |  | Used together with `DUMP`, this lets you specify the output format. Valid values are: json,xml,txt,csv,html.|
| **TO** | /K |  | Used together with `DUMP`, this lets you specify the outpath path.|
| **PURGEDB** | /O |  | Performs a database cleanup, if applicable.|
|  |  | *path/wildcard* | If a text value is specified, it can be the full name of a specific file or a path with * and ? wildcard support. If no value is specified, it removes all values for non-existing items (`KEEPMISSINGDRIVES` can change this behavior). |
| **KEEPMISSINGDRIVES** | /S |  | Used together with `PURGEDB` (without a specified value), it allows you to keep missing entries that are located on unavailable drives (e.g. on an external disk).|
| **VACUUM** | /S |  | Compacts the database. Use together with `PURGEDB`.|

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

#### **About cache**

The script includes optional built-in support that allows you to store data extracted with ExifTool in a database, so when you need it again, it can be retrieved easily and much faster.
This option is enabled by default and can be changed from the Options window, accessible from the Column Editor or by pressing the About button in the Opus Script Management.
To use this feature, you need to have the [Microsoft Access Database Engine](https://www.microsoft.com/en-us/download/details.aspx?id=54920) installed, which usually comes with Microsoft Office.
If the script detects that the engine is not installed, you will not be able to enable or use this optional feature.
Additionally, you can decide the minimum file size for files whose data will be read and stored in the database.
You can also decide whether this option should be available when Opus is running from a USB export.
There is also be a way to define exclusions with wildcard support, so you can ignore certain files and prevent them from being added to the database.

<img width="802" height="417" alt="database_options" src="https://github.com/user-attachments/assets/f3a2f30a-5387-4dc9-b0d0-78f5a776c812" />

---

#### **Notes:**

* ExifTool handles "human-readable" data in addition to conventional values. With this script, you get access to both, and you can choose between them per column, on runtime, by toggling the Use numeric value option.
* The script adds certain values to the "Self" group for convenience: `ErrorCount` and `WarningCount`. Those 2 values are handy in order to find files with corrupted metadata.
* The script adds some common predefined columns with suggested file types. Feel free to edit them, and you can still add more entries to the list whenever you drop a file into it.
* You can access to the Options dialog by clicking the About button from the Script Management window.
* For the database cache feature, the script can automatically detect most file changes and update their values in the database by using a file sampling technique that performs micro MD5 checksums. Note that for some plain text files, this technique may fail to detect changes.
* Columns with the same label will be modified internally to avoid duplicates.
* Custom columns can't be used for other custom columns.
* When using `OpusExifTool PURGEDB`, note that some paths (e.g. slow remote ones) may take longer to be checked for existence. During this time, the database will not be available.

---

#### **Acknowledgments/Credits:**

* [ExifTool by Phil Harvey](https://exiftool.org).
* [@PolarGoose](https://github.com/PolarGoose), for the awesome plugin that makes this project possible.
* Opus devs, for an amazing program that goes beyond a file manager.

---

