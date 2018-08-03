# Powershell Quick Reference
[See Official PowerShell Doc](https://docs.microsoft.com/en-us/powershell/scripting/powershell-scripting?view=powershell-6)

## Comments
```Powershell
# This is a comment
```

```Powershell
<# 
    Multiline
    Comment
#>
```
## Help
```Powershell
Get-Help Write-Host         # Show help for the command Write-Host
$PSVersionTable             # Show PowerShell and libraries versions
Get-Service | Get-Member    # Show all members of the Get-Service command
Get-Culture                 # Get the culture
```

## Comparison operators
```Powershell
# Equals                    |   -eq
# Not equal                 |   -ne
# Greater than              |   -gt
# Greater or equal than     |   -ge
# Less than                 |   -lt
# Less or equal than        |   -le

# Examples
5 -eq 5     # True
5 -lt 5     # False
```

## Booleans
```Powershell
# $True                 # True
# $False                # False
# $True -And $True      # True
# $True -Or $ False     # True
# -Not $True            # False

```

## Strings
```Powershell
"" | Get-Member                     # Show all operations on strings
"hello " + "bob"                    # Concatenate
"hello $variable"                   # We can put a variable inside a string
@("bob", "carl") -Join "-"          # Join an array to create a string. Returns "bob-carl"
"Hello World".Split(" ")            # Split a string into an array. Returns @("Hello", "World")
"Hello bob".Substring(6,3)          # Extract a substring (position, length). Returns "bob"
"Hello bob".Replace("bob", "carl")  # Replace "bob" by "carl"
"Hello bob".Contains("bob")         # Check if the string contains a substring
"Hello bob".IndexOf("bob")          # Position of the first occurence. Returns 6
"bob" -eq "carl"                    # Check if strings are equal
"bob" -ne "carl"                    # Check if strings are NOT equal
"bob" -Like "b?b"                   # Compare strings using wildcards
"bob" -eq "BOB"                     # Return True. Powershell is case insensitive. We add "c" to operator to be case sensitive
"bob" -ceq "BOB"                    # Return False
"bob" -clike "B?B"                  # Return False
```

## Conditional Statement
```Powershell
# If statement
if($True) 
{
    Write-Host "It is true"
}else 
{
    Write-Host "Nothing make sense"
}

# Switch
switch("denis")
{
    "carl"  {"This is Carl"}
    "bob"   {"This is Bob"}
    default {"who is this ?"}
}

# Switch without values in quotes
switch("denis")
{
    carl  {"This is Carl"}
    bob   {"This is Bob"}
    default {"who is this ?"}
}
```

## Files and folders
```Powershell
# Create a Folder
New-Item -ItemType Directory -Path "A new directory"

# Create a file
New-Item "newfile.txt"
New-Item -ItemType File -Path "newfile.txt"

# Check if file or folder exists
Test-Path "file.txt"

# Copy a file or folder
Copy-Item "newfile.txt" "newfile2.txt"

# Delete a file or folder
Remove-Item "newfile2.txt"          # File
Remove-Item "newFolder"  -Force     # Folder

# Read file content
Get-Content "test.txt"

# Clear file content
Clear-Content "text.txt"

# Write to file (overide content)
Set-Content "newfile.txt" "Hello"

# Append content to a file
Add-Content "newfile.txt" "World"               # Line feed
Add-Content "newfile.txt" "World" -NoNewLine    # No line feed
```

## Dates
```Powershell
Get-Date                                  # Show system date
Get-Date -Format "yyyy-MM-dd hh:mm:ss"    # Show formated system date
Get-Date -Year 2016 -Month 12 -Day 31     # Show a date
```

## Arrays
```Powershell
# Arrays are basic structures in PowerShell. It is fixed size only.
$a = "one", "two", "three"
$a = 1, 2, 3, 4
$a = 1..4                   # Same as previous line
$a = @()                    # Create an empty array
$a = @(1)                   # Create an array with only one element
$a.Count                    # Get the number of items in the array
$a[0]                       # Accessing one particular element
$a[0] = 100                 # Assign new value for element
$a.Clear                    # Empty the array
(0..9).Where{ $_ % 2 }      # Filter (return odd numbers)
```

## ArrayList (.Net)
```Powershell
# Better array with more capabilities
$t = [System.Collections.ArrayList]::new()      # Init array
[void]$t.add("bob")                             # [void] won't log in the console
[void]$t.add("carl")
$t.Contains("bob")                              # True
$t.Remove("bob")                                # Fairwell bob
$t.Count                                        # Count the number of elements
```

## Classes
```Powershell
class Car {   
    hidden [string]$Manufacturer                # Hides the member from being display. We can still use it. It is NOT private
    static [string]$TypeOfVehicule = "Car"      # Static member
    [ValidateNotNullOrEmpty()][int]$NbOfWheels  # Property validation
    
    # Constructor
    Car([string]$Manufacturer) {
        $this.Manufacturer = $Manufacturer
        $this.NbOfWheels = 4
    }
    [int]AddWheels([int]$WheelNumber){
        $This.NbOfWheels += $WheelNumber
        return $This.NbOfWheels
    }
}

$car = [Car]::new("Honda")
$car.AddWheels(2)
$car.NbOfWheels                 # Return 8
$car                            # Won't display Manufacturer, because it is hidden
$car.Manufacturer               # We can still display it's value when called directly
$car.Manufacturer = "Toyota"    # We can also change it's value even if it is hidden
[Car]::TypeOfVehicule           # Call a static value
```

## Objects
```Powershell

$obj = New-Object PSObject                      # Create a new PowerShell Object
$obj | Add-Member NoteProperty Name("Bob")      # Add property "Name" with value "Bob". NoteProperty is the default for custom ps obj

```

## Hashsets
```Powershell
$bank = @{}                 # Create an empty hashset (representing bank accounts)
$bank.Add("bob", 100)       # Add bob with 100$
$bank.Add("carl", 56)       # Add carl with 56$
$bank["bob"]                # Get bob's account amount (100)
$bank["bob"] = 200          # Change bob's amount
$bank = @{                  # Initialise and assign at the same time
    "bob" = 100
    "carl" = 56
    "ben" = 30
}
$bank[@("bob", "carl")]     # Select multiple keys at once (by using an array). Return 100 and 56


```

## Loops
```Powershell
# For loop
for($i=0; $i -lt 3; $i++)
{
    Write-Host $i
}

# Foreach
$l = "one", "two", "three"
foreach($Item in $l)
{
    Write-Host $Item
}

# Foreach (Shorthand)
$l | %{ Write-Host $_ }         # '%' means foreach and '$_' is the current item being iterated

# While
$i = 10
while($i -gt 0)
{
    Write-Host $i
    $i--
}

# Do While
$i = 10
do
{
    Write-Host $i
    $i--
}while($i -gt 0)
```

## Enums
```Powershell
enum Animal {
    Cat
    Dog
    Cow
}

[Animal].GetEnumNames()             # Get what is in the enum
[Animal].GetEnumName(1)             # Get the element with the value 1
```

## Using .Net library
```Powershell
$sb = [System.Text.StringBuilder]::new()

# [void] is used to hide output in the console
[void]$sb.Append( 'Was it a car ' )
[void]$sb.AppendLine( 'or a cat I saw?' )
$sb.ToString()
```

## Useful commands
```Powershell
Get-History                                                 # Show the commands that where typed in the console
Start-Process "notepad"                                     # Start a process
Stop-Process -Name "notepad"                                # Stopping a process
Start-Service -Name "eventlog"                              # Starting a service
Suspend-Service -Name "eventlog"                            # Pause a service
Stop-Service -Name "eventlog"                               # Stopping a service
Test-Connection localhost                                   # Ping
Get-Service | Where-Object { $_.Status -eq "Stopped" }      # Get all services where the status is "Stopped"
$p = get-process | convertto-csv                            # Convert and object to csv
$p | convertfrom-csv                                        # Convers csv to an object
Get-Process | Sort-Object -Property Id                      # Sort object
Get-Process | Sort-Object -Property CPU -Descending         # Sort object descending
Get-Random -Maximum 100                                     # Random value [0, 99]
Get-Random -Minimum -100 -Maximum 100                       # Random value [-100, 99]
Get-Process | Select-Object -First 5                        # Get first 5 items
Get-Process | Select-Object -Last 5                         # Get last 5 items
1,2,2,3 | Select-Object -Unique                             # Select only the unique objects. Returns @(1, 2, 3)
Select-String newfile.txt -Pattern "b*"                     # Return the lines from a file that match a regex
Select-String *.txt -Pattern "bat"                          # Search for "bat" in all .txt files
Get-Childitem                                               # Same as command "Dir"
Get-ChildItem -Recurse | Select-String -Pattern "bob"       # Search for a string in a folder and all sub folders
Get-Process | Tee-Object -FilePath output.txt               # Output the command Get-Process into a file
Write-Progress                                              # Show progressbar in the console
Get-Content "t.txt" | Measure-Object -Character -Line -Word # Calculate the Number of characters, lines and word in a file
```
