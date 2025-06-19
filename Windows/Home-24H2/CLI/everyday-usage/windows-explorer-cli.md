# 📁 Interacting with Windows Explorer from CLI

Explore useful ways to interact with or extract insights from Windows Explorer-managed directories, directly from the command line.

This section focuses on bridging GUI usage with the CLI — extracting data, analyzing folders, and scripting behaviors that otherwise require GUI-based exploration.

---

## 🔍 Get Folder Sizes Sorted – PowerShell Example

> Windows Explorer doesn't always show folder sizes, especially for nested folders — making it tedious to understand disk usage at a glance. This PowerShell snippet solves that pain point by recursively calculating folder sizes and presenting them in a sorted, CLI-friendly format.

### CLI command:

```shell
powershell "Get-ChildItem -Directory | ForEach-Object { $size = [math]::Ceiling((Get-ChildItem $_.FullName -Recurse | Measure-Object -Property Length -Sum).Sum / 1MB); [PSCustomObject]@{Name=$_.Name; SizeMB=$size; FullName=$_.FullName} } | Sort-Object SizeMB -Descending"
```

### output:

```text
STDOUT: Name                                SizeMB FullName
STDOUT: ----                                ------ --------
STDOUT: OptGuideOnDeviceModel                 3021 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\OptGuideOnDeviceModel
STDOUT: Profile 2                              920 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\Profile 2
STDOUT: Profile 4                              612 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\Profile 4
STDOUT: Profile 6                              165 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\Profile 6
STDOUT: Default                                118 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\Default
STDOUT: component_crx_cache                    116 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\component_crx_cache
STDOUT: optimization_guide_model_store         114 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\optimization_guide_model_store
STDOUT: screen_ai                              107 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\screen_ai
STDOUT: System Profile                          42 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\System Profile
STDOUT: Safe Browsing                           41 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\Safe Browsing
STDOUT: Snapshots                               35 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\Snapshots
STDOUT: GrShaderCache                           29 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\GrShaderCache
STDOUT: SwReporter                              26 C:\Users\suhas\AppData\Local\Google\Chrome\User Data\SwReporter
...
```

return/exit code: 0

---

> This is one example of surfacing file system usage patterns directly from a CLI interface. More directory and file-level Windows Explorer behaviors can be scripted and recorded similarly.
