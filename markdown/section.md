# Section Object (JavaScript API for OneNote)

_Applies to: OneNote Online_  
_Note: This API is in preview_  


Represents a OneNote section. Sections can contain pages.

## Properties

| Property	   | Type	|Description|Feedback|
|:---------------|:--------|:----------|:-------|
|clientUrl|string|The client url of the section. Read only Read-only.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-clientUrl)|
|id|string|Gets the ID of the section. Read-only.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-id)|
|name|string|Gets the name of the section. Read-only.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-name)|

_See property access [examples.](#property-access-examples)_

## Relationships
| Relationship | Type	|Description| Feedback|
|:---------------|:--------|:----------|:-------|
|notebook|[Notebook](notebook.md)|Gets the notebook that contains the section. Read-only.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-notebook)|
|pages|[PageCollection](pagecollection.md)|The collection of pages in the section. Read only Read-only.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-pages)|
|parentSectionGroup|[SectionGroup](sectiongroup.md)|Gets the section group that contains the section. Throws ItemNotFound if the section is a direct child of the notebook. Read-only.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-parentSectionGroup)|
|parentSectionGroupOrNull|[SectionGroup](sectiongroup.md)|Gets the section group that contains the section. Returns null if the section is a direct child of the notebook. Read-only.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-parentSectionGroupOrNull)|

## Methods

| Method		   | Return Type	|Description| Feedback|
|:---------------|:--------|:----------|:-------|
|[addPage(title: string)](#addpagetitle-string)|[Page](page.md)|Adds a new page to the end of the section.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-addPage)|
|[copyToNotebook(destinationNotebook: Notebook)](#copytonotebookdestinationnotebook-notebook)|[Section](section.md)|Copies this section to specified notebook.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-copyToNotebook)|
|[copyToSectionGroup(destinationSectionGroup: SectionGroup)](#copytosectiongroupdestinationsectiongroup-sectiongroup)|[Section](section.md)|Copies this section to specified section group.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-copyToSectionGroup)|
|[insertSectionAsSibling(location: string, title: string)](#insertsectionassiblinglocation-string-title-string)|[Section](section.md)|Inserts a new section before or after the current section.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-insertSectionAsSibling)|
|[load(param: object)](#loadparam-object)|void|Fills the proxy object created in JavaScript layer with property and object values specified in the parameter.|[Go](https://github.com/OfficeDev/office-js-docs/issues/new?title=OneNote-section-load)|

## Method Details


### addPage(title: string)
Adds a new page to the end of the section.

#### Syntax
```js
sectionObject.addPage(title);
```

#### Parameters
| Parameter	   | Type	|Description|
|:---------------|:--------|:----------|
|title|string|The title of the new page.|

#### Returns
[Page](page.md)

#### Examples
```js
OneNote.run(function (context) {
            
    // Queue a command to add a page to the current section.
    var page = context.application.getActiveSection().addPage("Wish list");
            
    // Queue a command to load the id and title of the new page. 
    // This example loads the new page so it can read its properties later.           
    page.load('id,title');
            
    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {
             
            // Display the properties.       
            console.log("Page name: " + page.title);
            console.log("Page ID: " + page.id);

        });
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```


### copyToNotebook(destinationNotebook: Notebook)
Copies this section to specified notebook.

#### Syntax
```js
sectionObject.copyToNotebook(destinationNotebook);
```

#### Parameters
| Parameter	   | Type	|Description|
|:---------------|:--------|:----------|
|destinationNotebook|Notebook|The notebook to copy this section to.|

#### Returns
[Section](section.md)

#### Examples
```js
OneNote.run(function (context) {
	var app = context.application;
	
	// Gets the active Notebook.
	var notebook = app.getActiveNotebook();
	
	// Gets the active Section.
	var section = app.getActiveSection();
	
	var newSection;
	
	return context.sync()
		.then(function() {
			newSection = section.copyToNotebook(notebook);
			newSection.load('id');
			return context.sync();
		})
		.then(function() {
			console.log(newSection.id);
		});
})
.catch(function (error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```


### copyToSectionGroup(destinationSectionGroup: SectionGroup)
Copies this section to specified section group.

#### Syntax
```js
sectionObject.copyToSectionGroup(destinationSectionGroup);
```

#### Parameters
| Parameter	   | Type	|Description|
|:---------------|:--------|:----------|
|destinationSectionGroup|SectionGroup|The section group to copy this section to.|

#### Returns
[Section](section.md)

#### Examples
```js
OneNote.run(function (ctx) {
	var app = ctx.application;
	
	// Gets the active Notebook.
	var notebook = app.getActiveNotebook();
	
	// Gets the active Section.
	var section = app.getActiveSection();
	
	var newSection;
	
	return ctx.sync()
		.then(function() {
			var firstSectionGroup = notebook.sectionGroups.items[0];
			newSection = section.copyToSectionGroup(firstSectionGroup);
			newSection.load('id');
			return ctx.sync();
		})
		.then(function() {
			console.log(newSection.id);
		});
})
.catch(function (error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```


### insertSectionAsSibling(location: string, title: string)
Inserts a new section before or after the current section.

#### Syntax
```js
sectionObject.insertSectionAsSibling(location, title);
```

#### Parameters
| Parameter	   | Type	|Description|
|:---------------|:--------|:----------|
|location|string|The location of the new section relative to the current section.  Possible values are: Before, After|
|title|string|The name of the new section.|

#### Returns
[Section](section.md)

#### Examples
```js
OneNote.run(function (context) {
            
    // Queue a command to insert a section after the current section.
    var section = context.application.getActiveSection().insertSectionAsSibling("After", "New section");
            
    // Queue a command to load the id and name of the new section. 
    // This example loads the new section so it can read its properties later.           
    section.load('id,name');
            
    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {
             
            // Display the properties.       
            console.log("Section name: " + section.name);
            console.log("Section ID: " + section.id);
        });
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```


### load(param: object)
Fills the proxy object created in JavaScript layer with property and object values specified in the parameter.

#### Syntax
```js
object.load(param);
```

#### Parameters
| Parameter	   | Type	|Description|
|:---------------|:--------|:----------|
|param|object|Optional. Accepts parameter and relationship names as delimited string or an array. Or, provide [loadOption](loadoption.md) object.|

#### Returns
void
### Property access examples

**id**
```js
OneNote.run(function (context) {
        
    // Get the current section.
    var section = context.application.getActiveSection();
            
    // Queue a command to load the section. 
    // For best performance, request specific properties.           
    section.load("id");
            
    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {
            console.log("Section ID: " + section.id);
        });
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

**name and notebook**
```js
OneNote.run(function (context) {
        
    // Get the current section.
    var section = context.application.getActiveSection();
            
    // Queue a command to load the section with the specified properties. 
    section.load("name,notebook/name");
            
    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {
            console.log("Section name: " + section.name);
            console.log("Parent notebook name: " + section.notebook.name);
        });
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

**parentSectionGroupOrNull**
```js
OneNote.run(function (context) {
    // Queue a command to add a page to the current section.
    var section = context.application.getActiveSection();
	section.load('clientUrl,notebook');
	var sectionGroup = section.parentSectionGroupOrNull;
    
    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {
			if(sectionGroup.isNull === false)
			{
				// If a parent section group exists, queue a command to add a section in it!
				sectionGroup.addSection("NewSectionInSectionGroup");
			}
			return context.sync();
        });
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```
	
