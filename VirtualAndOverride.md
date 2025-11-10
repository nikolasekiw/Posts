# Use of virtual and override in methods
> Practical examples of how and why virtual and override can be useful in c#

### Virtual method in a base class
A virtual method allows derived classes to extend or modify its behavior. Here’s an example of a base class method that sets up an Excel header:
```csharp
protected virtual void CreateExcelHeader(ExcelWorksheet ws)
{ 
    ws.Cells[1, 1].Value = Resources.PageName;
    //additional setup code
}
```
### Overriding in a derived class
A derived class can override the virtual method to add more specific functionality while still calling the base implementation:
```csharp
protected override void CreateExcelHeader(ExcelWorksheet ws)
{
    base.CreateExcelHeader(ws);
    ws.Cells[ws.Dimension?.End.Row ?? 0, ws.Dimension?.End.Column+1 ?? 0].Value = Resources.Unit;
    //additional customization
}
```

### Why this is useful
By making CreateExcelHeader virtual, we can extend and personalize its behavior in subclasses. This is especially helpful when different subclasses need slightly different setups.

### Before using virtual and override
Previously, the method was not virtual, so additional code had to be written outside the method call:
```csharp
ExcelWorksheet ws = package.Workbook.Worksheet.Add(Resources.PublishedPages);
CreateExcelHeader(ws);
ws.Cells[] //more code here
```
#### After overriding the virtual method
Now, all necessary logic is encapsulated within the overridden method:
```csharp
ExcelWorksheet ws = package.Workbook.Worksheet.Add(Resources.PublishedPages);
CreateExcelHeader(ws);
```
This approach is much cleaner and more maintainable. The code that was previously outside the method was tightly coupled with the logic inside CreateExcelHeader, so it makes sense to keep them together. Npw, calling a single method handles everything, making the code easier to read and reuse. 
