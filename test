# Check for admin privileges
if (-not ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Write-Host "Requesting administrator privileges..."
    Start-Process -FilePath "powershell.exe" -ArgumentList "-ExecutionPolicy Bypass -File `"$PSCommandPath`"" -Verb RunAs
    exit
}

# Check current edition
$editionInfo = DISM /Online /Get-CurrentEdition 2>$null | Where-Object { $_ -match "Current Edition" }
$currentEdition = ($editionInfo -split ":")[1].Trim()

# Exit if already Professional
if ($currentEdition -ieq "Professional") {
    exit
}

# Set the Pro key
$proKey = "VK7JG-NPHTM-C97JM-9MPGT-3V66T"

# Try to upgrade with DISM
$dismResult = DISM /Online /Set-Edition:Professional /ProductKey:$proKey /AcceptEula 2>$null

if ($LASTEXITCODE -ne 0) {
    # Try using slmgr.vbs if DISM fails
    cscript //nologo "$env:SystemRoot\System32\slmgr.vbs" /ipk $proKey > $null
    cscript //nologo "$env:SystemRoot\System32\slmgr.vbs" /ato > $null
}
