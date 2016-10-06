# IntelHexFormatReader #

A .NET library to parse [an Intel HEX file](https://en.wikipedia.org/wiki/Intel_HEX) (often used for representing code used to program microprocessors and microcontrollers) into a representative "memory representation".

## How to use the .NET library##

Link the following nuget package in your project in order to use the Intel HEX format reader: https://www.nuget.org/packages/IntelHexFormatReader/

Alternatively, install the package using the nuget package manager console:

```
Install-Package IntelHexFormatReader
```

### 1. Using HexFileReader ###

The following minimal snippet demonstrates loading an Intel HEX file and printing out it's resulting memory representation:

```csharp
using System.Diagnostics;
using IntelHexFormatReader;

namespace IntelHexFormatReaderDemo
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            HexFileReader reader = new HexFileReader(@"C:\\MyHexFiles\\myHexFile.hex", 32768);
            MemoryBlock memoryRepresentation = reader.Parse();
            foreach (MemoryCell cell in memoryRepresentation.Cells)
                Debug.WriteLine(cell);
        }
    }
}
```
This will resulting in output which might look like this:
```
MemoryCell : 00000000 Value: 0C (modified = True)
MemoryCell : 00000001 Value: 94 (modified = True)
MemoryCell : 00000002 Value: 72 (modified = True)
MemoryCell : 00000003 Value: 00 (modified = True)
MemoryCell : 00000004 Value: 0C (modified = True)
MemoryCell : 00000005 Value: 94 (modified = True)
MemoryCell : 00000006 Value: 9A (modified = True)
MemoryCell : 00000007 Value: 00 (modified = True)
MemoryCell : 00000008 Value: 0C (modified = True)
MemoryCell : 00000009 Value: 94 (modified = True)
MemoryCell : 0000000A Value: 9A (modified = True)
MemoryCell : 0000000B Value: 00 (modified = True)
MemoryCell : 0000000C Value: 0C (modified = True)
MemoryCell : 0000000D Value: 94 (modified = True)
MemoryCell : 0000000E Value: 9A (modified = True)
MemoryCell : 0000000F Value: 00 (modified = True)
...
```

Please note tat ```HexFileReader``` has a second constructor which allows you to pass any ```IEnumerable<string>``` directly.

### 2. Using HexFileLineParser ###

Alternatively, if you have only a single HEX record line to parse, you can use ```HexFileLineParser.ParseLine``` directly:

```csharp
string hexRecordLine = ":100130003F0156702B5E712B722B732146013421C7";
IntelHexRecord record = HexFileLineParser.ParseLine(hexRecordLine);
Debug.WriteLine(record.ByteCount);
Debug.WriteLine(record.Address.ToString("X4"));
Debug.WriteLine(record.RecordType.ToString());
Debug.WriteLine(record.Bytes[5]);
Debug.WriteLine(record.Bytes[9]);
Debug.WriteLine(record.CheckSum);
```

This will result in the following output:

```
16
0130
Data
94
43
199
```

The ```HexFileLineParser``` does fairly exhaustive validation so you should not be able to construct invalid ```IntelHexRecord``` objects.

### Currently Unimplemented ###

* Record Type ```StartSegmentAddress```
* Record Type ```StartLinearAddress```
