# Custom Application Integration Export

Cyncly Design Live (formerly 2020 Design) allows custom exports through **Application Integration**. This feature lets you create your own export option that appears directly in the application without requiring any modifications to the software itself.

A custom export consists of three files placed in the Queue directory:

```text
C:\ProgramData\Cyncly\Design\<Version>\DB\Queue
```

**Example (Version 14):**

```text
C:\ProgramData\Cyncly\Design\14\DB\Queue
```

---

## Folder Structure

```text
Queue/
├── CustomExport.que
├── CustomVariableList.xml
└── ThirdParty.exe          (Optional)
```

### `CustomExport.que`

This file registers your custom export with Cyncly Design Live.

It contains three lines:

```text
Display Name
Executable
Variable List
```

Example:

```text
Advanced Export
ThirdParty.exe
CustomVariableList.xml
```

| Line | Description |
| ---- | ----------- |
| **1** | Display name shown in the Application Integration export list. |
| **2** | Executable to launch after the XML export is generated. Leave this line blank if no executable is required. |
| **3** | Variable list XML that controls which variables are included or excluded from the export. |

If no executable is specified, Cyncly Design Live will still generate the XML export.

---

### `CustomVariableList.xml`

This file controls which built-in variables are exported.

Example:

```xml
<VarList active="1">
    <Var exclude="0" ID="-38"/>
    <Var exclude="0" ID="1235"/>
    <Var exclude="0" ID="326"/>
    <Var exclude="0" ID="830"/>
    <Var exclude="0" ID="1748"/>
    <Var exclude="0" ID="1315"/>
</VarList>
```

#### Attributes

| Attribute | Description |
| --------- | ----------- |
| `active="1"` | Enables the variable list. |
| `ID` | The internal Cyncly variable ID. |
| `exclude` | Controls whether the variable is exported. |

### `exclude`

| Value | Meaning |
| ----: | ------- |
| `0` | Include (export) this variable. |
| `1` | Exclude this variable. |

> **Note:** Variable selection appears to be based solely on the internal Cyncly variable ID.

---

### `ThirdParty.exe` (Optional)

This executable is completely optional.

If specified in the `.que` file, Cyncly Design Live launches it **after** generating the export XML.

The path to the generated XML file is passed to the executable as the first command-line argument.

Example:

```text
ThirdParty.exe
```

This allows you to:

- Parse the XML with a custom viewer.
- Import the XML into another application.
- Convert the XML into another format.
- Upload the export to a web service.
- Perform additional automation.

If no executable is provided, Cyncly Design Live will still generate the XML export using the custom variable list.

---

## Complete Example

```text
Queue/
├── AdvancedExport.que
├── CustomVariableList.xml
└── MyExporter.exe
```

**AdvancedExport.que**

```text
Advanced Export
MyExporter.exe
CustomVariableList.xml
```

**CustomVariableList.xml**

```xml
<VarList active="1">
    <Var exclude="0" ID="-38"/>
    <Var exclude="0" ID="1235"/>
    <Var exclude="0" ID="326"/>
</VarList>
```

When the user selects **Advanced Export** from Application Integration:

1. Cyncly Design Live generates the XML export.
2. Only the variables defined by `CustomVariableList.xml` are included (or excluded, depending on the `exclude` attribute).
3. If specified, `MyExporter.exe` is launched and the path to the generated XML file is passed as the first command-line argument.

---

## Notes

- Place all required files in the `Queue` directory.
- Multiple `.que` files can exist, allowing multiple custom export options.
- An executable is **not required**; XML generation works without one.
  - If no executable is specified, Cyncly Design Live displays a **Save As** dialog prompting you to choose where to save the XML file. The XML file name will initially be blank.
  - If an executable is specified, Cyncly Design Live automatically saves the XML using the same name and location as the source `.kit` file.
    - For example, `C:\Designs\Test.kit` will automatically generate `C:\Designs\Test.xml`.
- Variable filtering uses the internal Cyncly variable IDs.
- This feature is built into Cyncly Design Live and does not require modifying the application.
- You can modify any of these files while Cyncly Design Live is running. Simply close and reopen the **Application Integration** window, and any changes will be loaded automatically.
