# Migration Step – Renaming properties and content types

When working with Optimizely, it's important to handle the renaming of properties and content types carefully to avoid data loss.

### Content types with GUIDs
If your content type (such as a block or page) has a GUID specified using the `[ContentType(GUID = "...")]` attribute, renaming the class is safe and does **not** require a migration step. Optimizely uses the GUID to track the content type, so changing the class name won't affect the stored data.
However, if no GUID is specified, or if you're renaming a **property** within a content type (since properties don't have GUIDs), you **must** use a migration step to ensure that existing data is preserved. Here's how you can do that:

### Migration Step
If you rename a property or content type programmatically, Optimizely will treat it as a **new field**, and any data stored under the old name will be lost in the CMS interface.

To preserve existing data, you should implement a `MigrationStep`. Migration steps are part of the `EPiServer.DataAbstraction.Migration` namespace.
It is a special class where you explicitly tell Optimizely:
- What the **new name** is
- What the **old name** was

This ensures that the old data is correctly mapped to the new field.  

### Example: Renaming a property
```csharp
private void RenameProperty()
{
    ContentType("Car")
        .Property("Color")
        .UsedToBeNamed("CarColor");
}
```
This tells Optimizely that the property `Color` on the `Car` content type was previously named `CarColor`. This way, any existing data under `CarColor` will be preserved and accessible under the new name.

### Example: Renaming a content type (e.g., block or page)
```csharp
private void RenameContentType()
{
    ContentType("Vehicle")
        .UsedToBeNamed("Car");
}
```
This ensures that the content type previously known as `Car` is now recognized as `Vehicle`, and all existing content will be retained.
