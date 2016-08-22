**contents**
```js
OneNote.run(function (context) {

    // Gets the active page.
    var activePage = context.application.getActivePage();

    // Queue a command to add a new page after the active page. 
    var pageContents = activePage.contents;

    // Queue a command to load the pageContents to access its data.
    context.load(pageContents);

    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function() {
            for(var i=0; i < pageContents.items.length; i++)
            {
                var pageContent = pageContents.items[i];
                if (pageContent.type == "Outline")
                {
                    console.log("Found an outline");
                }
                else if (pageContent.type == "Image")
                {
                    console.log("Found an image");
                }
                else if (pageContent.type == "Other")
                {
                    console.log("Found a type not supported yet.");
                }
            }
        });
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

**webUrl**
```js
OneNote.run(function (context) {

	var app = context.application;
	
	// Gets the active page.
	var page = app.getActivePage();
	
	// Queue a command to load the webUrl of the page.
	page.load("webUrl");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function() {
			console.log(page.webUrl);
		});
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

**inkAnalysisOrNull**
```js
OneNote.run(function (ctx) {		
	var app = ctx.application;
	
	// Gets the active page.
	var page = app.getActivePage();
	
	// Load ink words
	page.load('inkAnalysisOrNull/paragraphs/lines/words');
	
	return ctx.sync()
		.then(function() {
			if (!page.inkAnalysisOrNull.isNull)
				console.log(page.inkAnalysisOrNull.paragraphs.length);
		});
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

### addOutline(left: double, top: double, html: String)
```js
OneNote.run(function (context) {

    // Gets the active page.
    var page = context.application.getActivePage();

    // Queue a command to add an outline with given html. 
    var outline = page.addOutline(200, 200,
"<p>Images and a table below:</p> \
 <img src=\"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==\"> \
 <img src=\"http://imagenes.es.sftcdn.net/es/scrn/6653000/6653659/microsoft-onenote-2013-01-535x535.png\"> \
 <table> \
   <tr> \
     <td>Jill</td> \
     <td>Smith</td> \
     <td>50</td> \
   </tr> \
   <tr> \
     <td>Eve</td> \
     <td>Jackson</td> \
     <td>94</td> \
   </tr> \
 </table>"     
        );

    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .catch(function(error) {
            console.log("Error: " + error);
            if (error instanceof OfficeExtension.Error) {
                console.log("Debug info: " + JSON.stringify(error.debugInfo));
            }
		});
});
```

### insertPageAsSibling(location: string, title: string)
```js
OneNote.run(function (context) {

    // Gets the active page.
    var activePage = context.application.getActivePage();

    // Queue a command to add a new page after the active page. 
    var newPage = activePage.insertPageAsSibling("After", "Next Page");

    // Queue a command to load the newPage to access its data.
    context.load(newPage);

    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function() {
            console.log("page is created with title: " + newPage.title);
        });
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

### copyToSection(destinationSection: Section)
```js
OneNote.run(function(ctx) {
	var app = ctx.application;
	
	// Gets the active notebook.
	var notebook = app.getActiveNotebook();
	
	// Gets the active page.
	var page = app.getActivePage();
	
	// Queue a command to load sections under the notebook.
	notebook.load('sections');
	
	var newPage;
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync()
		.then(function() {
			var section = notebook.sections.items[0];
			
			// copy page to the section.
			newPage = page.copyToSection(section);
			newPage.load('id');
			return ctx.sync();
		})
		.then(function() {
			console.log(newPage.id);
		});
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```
