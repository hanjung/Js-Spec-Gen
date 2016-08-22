### Getter

**id and type**
```js
OneNote.run(function (context) {

    // Get the collection of pageContent items from the page.
    var pageContents = context.application.getActivePage().contents;
    
    // Queue a command to load the outline property of each pageContent.
    pageContents.load("outline");
        
    // Get the first PageContent on the page, and then get its Outline.
    var pageContent = pageContents._GetItem(0);
    var paragraphs = pageContent.outline.paragraphs;
            
    // Queue a command to load the id and type of each paragraph.
    paragraphs.load("id,type");
            
    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {
            
            // Write the text.                  
            $.each(paragraphs.items, function(index, paragraph) {
                console.log("Paragraph type: " + paragraph.type);
                console.log("Paragraph ID: " + paragraph.id);
            });
        });
})		
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
}); 
```

**paragraphs**
```js
OneNote.run(function(context) {
	var app = context.application;
	
	// Gets the active outline
	var outline = app.getActiveOutline();
	
	// load nested paragraphs and their types.
	outline.load("paragraphs/type");
	
	return context.sync().then(function () {
		var paragraphs = outline.paragraphs.items;
		
		var promise;
		// for each nested paragraphs, load tables only
		for (var i = 0; i < paragraphs.length; i++) {
			var paragraph = paragraphs[i];
			if (paragraph.type == "Table") {
				paragraph.load("table/id");
				promise =  context.sync().then(function() {
					console.log(paragraph.table.id);
				});
			}
		}
		return promise;
	})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

### delete()
```js
OneNote.run(function (context) {

    // Get the collection of pageContent items from the page.
    var pageContents = context.application.getActivePage().contents;

    // Get the first PageContent on the page
    // Assuming its an outline, get the outline's paragraphs.
    var pageContent = pageContents.getItemAt(0);
	
    var paragraphs = pageContent.outline.paragraphs;
	
	var firstParagraph = paragraphs.getItemAt(0);
	
    // Queue a command to load the id and type of the first paragraph
    firstParagraph.load("id,type");

    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {
			
            // Queue a command to delete the first paragraph                 
            firstParagraph.delete();
			
			// Run the command to delete it
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

### insertHtmlAsSibling(insertPosition: string, html: string)
```js
OneNote.run(function (context) {

	// Get the collection of pageContent items from the page.
	var pageContents = context.application.getActivePage().contents;

	// Get the first PageContent on the page
	// Assuming its an outline, get the outline's paragraphs.
	var pageContent = pageContents.getItemAt(0);
	var paragraphs = pageContent.outline.paragraphs;
	var firstParagraph = paragraphs.getItemAt(0);

	// Queue a command to load the id and type of the first paragraph
	firstParagraph.load("id,type");

	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function () {

			// Queue commands to insert before and after the first paragraph
			firstParagraph.insertHtmlAsSibling("Before", "<p>ContentBeforeFirstParagraph</p>");
			firstParagraph.insertHtmlAsSibling("After", "<p>ContentAfterFirstParagraph</p>");
			
			// Run the command to run inserts
			return context.sync();
		});
))
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

### insertImageAsSibling(insertLocation: string, base64EncodedImage: string, width: double, height: double)
```js
OneNote.run(function (context) {

	// Get the collection of pageContent items from the page.
	var pageContents = context.application.getActivePage().contents;

	// Get the first PageContent on the page
	// Assuming its an outline, get the outline's paragraphs.
	var pageContent = pageContents.getItemAt(0);
	var paragraphs = pageContent.outline.paragraphs;
	var firstParagraph = paragraphs.getItemAt(0);

	// Queue a command to load the id and type of the first paragraph
	firstParagraph.load("id,type");

	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function () {

			// Queue commands to insert before and after the first paragraph
			firstParagraph.insertImageAsSibling("Before", "R0lGODlhDwAPAKECAAAAzMzM/////wAAACwAAAAADwAPAAACIISPeQHsrZ5ModrLlN48CXF8m2iQ3YmmKqVlRtW4MLwWACH+H09wdGltaXplZCBieSBVbGVhZCBTbWFydFNhdmVyIQAAOw==");
			firstParagraph.insertImageAsSibling("After", "R0lGODlhDwAPAKECAAAAzMzM/////wAAACwAAAAADwAPAAACIISPeQHsrZ5ModrLlN48CXF8m2iQ3YmmKqVlRtW4MLwWACH+H09wdGltaXplZCBieSBVbGVhZCBTbWFydFNhdmVyIQAAOw==");
			
			// Run the command to insert images
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

### insertRichTextAsSibling(insertLocation: string, paragraphText: string)
```js
OneNote.run(function (context) {

	// Get the collection of pageContent items from the page.
	var pageContents = context.application.getActivePage().contents;

	// Get the first PageContent on the page
	// Assuming its an outline, get the outline's paragraphs.
	var pageContent = pageContents.getItemAt(0);
	var paragraphs = pageContent.outline.paragraphs;
	var firstParagraph = paragraphs.getItemAt(0);

	// Queue a command to load the id and type of the first paragraph
	firstParagraph.load("id,type");

	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function () {

			// Queue commands to insert before and after the first paragraph
			firstParagraph.insertRichTextAsSibling("Before", "Text Appears Before Paragraph");
			firstParagraph.insertRichTextAsSibling("After", "Text Appears After Paragraph");
			
			// Run the command to insert text contents
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

### select()
```js
OneNote.run(function (context) {

	// Get the collection of pageContent items from the page.
	var pageContents = context.application.getActivePage().contents;

	// Get the first PageContent on the page
	// Assuming its an outline, get the outline's paragraphs.
	var pageContent = pageContents.getItemAt(0);
	var paragraphs = pageContent.outline.paragraphs;

	var firstParagraph = paragraphs.getItemAt(0);

	// Queue a command to load the id and type of the first paragraph
	firstParagraph.load("id,type");

	// Queue a command to select the first paragraph  
	firstParagraph.select();

	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function () {
			console.log("Selected paragraph with id : " + firstParagraph.id + " and type: " + firstParagraph.type);
			return Promise.resolve(firstParagraph);
		});
})		
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
}); 
```
