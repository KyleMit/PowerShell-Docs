---
description: Describes automatic members in all PowerShell objects
Locale: en-US
ms.date: 11/16/2021
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_Inrinsic_Members?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Intrinsic_Members
---

# About intrinsic members

## Short description

Provides information about PowerShell's intrinsic members that are available to
all PowerShell objects.

## Detailed description

When objects are created, PowerShell adds some "hidden" properties and methods
to each object. These properties and methods are known as _intrinsic members_.
These intrinsic members are normally hidden from view. Some of these members
can be seen using the `Get-Member -Force` command.

## Object views

The intrinsic members include a set of **MemberSet** properties that represent
a view of the object. You can find the **MemberSet** properties using the
`Get-Member -Force` command on any PowerShell object. Every PowerShell object
includes the following **MemberSet** properties.

### psbase

This **psbase** contains the members the base object without extension or
adaptation.

### psadapted

The **psadapted** view shows the base object plus the adapted members, if
present. Adapted members are added by the Extended Type System (ETS).

### psextended

The **psextended** view _only_ shows the members added by the
[Types.ps1xml](about_Types.ps1xml.md) files and the
[Add-Member](xref:Microsoft.PowerShell.Utility.Add-Member) cmdlet. Any object
can be extended at runtime using the `Add-Member` cmdlet.

### psobject

The base type of all PowerShell objects is `[PSObject]`. However, when an
object gets created, PowerShell also wraps the object with a `[PSObject]`
instance. The **psobject** member allows access to the `[PSObject]` wrapper
instance. The wrapper includes methods, properties, and other information about
the object. Using the **psobject** member is comparable to using
[Get-Member](xref:Microsoft.PowerShell.Utility.Get-Member), but there are some
differences since it is only accessing the wrapper instance.

## Type information

### pstypenames

**PSTypeNames** is a **CodeProperty** member that lists the object type
hierarchy in order of inheritance. For example:

```powershell
$file = Get-Item C:\temp\test.txt
$file.pstypenames
```

```Output
System.IO.FileInfo
System.IO.FileSystemInfo
System.MarshalByRefObject
System.Object
```

As shown above, it starts with the most specific object type,
`System.IO.FileInfo`, and continues down to the most generic type,
`System.Object`.

## Methods

PowerShell adds two hidden methods to all PowerShell objects. These methods are
not visible using the `Get-Member -Force` command or tab completion.

### ForEach() and Where()

The `ForEach()` and `Where()` methods are available to all PowerShell
objects. However, they are most useful when working with collections. For more
information on how to use these methods, see [about_Arrays](about_Arrays.md).

## Properties

### Count and Length

The **Count** and **Length** properties are available to all PowerShell
objects. These are similar to each other but may work differently depending on
the data type. For more information about these properties, see
[about_Properties](about_Properties.md).

## Array indexing scalar types

When an object is not an indexed collection, using the index operator to access
the first element returns the object itself. Index values beyond the first
element return `$null`.

```
PS> (2)[0]
2
PS> (2)[-1]
2
PS> (2)[1] -eq $null
True
PS> (2)[0,0] -eq $null
True
```

For more information, see [about_Operators](about_Operators.md#index-operator--).

## New() method for types

Beginning in PowerShell 5.0, PowerShell adds a static `New()` method for all
.NET types. The following examples produce the same result.

```powershell
$expression = New-Object -TypeName regex -ArgumentList 'pattern'
$expression = [regex]::new('pattern')
```

Using the `new()` method performs better than using `New-Object`.

For more information, see [about_Classes](about_Classes.md).
