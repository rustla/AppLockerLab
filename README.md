# AppLocker Playground
An intentionally vulnerable AppLocker policy used to demonstrate several allowlisting misconfigurations.

# Usage
1. Open `Local Security Policy` in a Windows VM - preferrably Server OS (Enterprise needed for workstation OS variants)
2. Import the `AppLockerPlayground.xml` into the Application Control Policies section
3. Run `gpupdate /force`
4. Run `net start appidsvc` as an administrative user
5. Login as a user with standard privileges

# Policy Build Notes
The policy was built using [AaronLocker](https://github.com/microsoft/AaronLocker), then lightly customised.
1. `.\Create-Policies.ps1`
2. Added `"C:\ProgramData\*"` to `.\CustomizationInputs\GetSafePathsToAllow.ps1`
3. Added the following to `.\CustomizationInputs\UnsafePathsToBuildRulesFor.ps1`:
```
# Permit any signed files required by VMware
@{
label = "VMware Tools";
paths = "C:\Program Files\VMware";
pubruleGranularity = "pubOnly";
}
```
4. Removed OneDrive path from `.\CustomizationInputs\UnsafePathsToBuildRulesFor.ps1`
5. Re-scanned with customised inputs `.\Create-Policies.ps1 -rescan`
6. Imported the policy using Local Security Policy to a fresh VM
7. Removed ~20 rules related to OneDrive
8. Added a publisher rule permitting MSBuild
9. Exported the policy to XML