### Getter

**items**
```js
OneNote.run(function (context) {

    // Get the collection of pageContent items from the page.
    var pageContents = context.application.getActivePage().contents;

    // Queue a command to load the type of each pageContent.
    pageContents.load("type");

    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {

            $.each(pageContents.items, function(index, pageContent) {
                console.log("PageContent type: " + pageContent.type);
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

**traverse for outlines**
```js
OneNote.run(function (context) {
   var page = context.application.getActivePage();
   var pageContents = page.contents;
   pageContents.load('type');
   var outlines = [];
   return context.sync()
	   .then(function () {	  
			  $.each(pageContents.items, function (index, pageContent) {
					 console.log(pageContent.type);
					 if (pageContent.type === 'Outline') {
						   outlines.push(pageContent);
					 }
			  });
			  $.each(outlines, function (index, outline) {
					 outline.load("id,paragraphs,paragraphs/type");
			  });
			  return context.sync();
	   })
	   .then(function () {
			  $.each(outlines, function (index, outline) {
					 console.log("An outline was found with id : " + outline.id);
			  });
			  return Promise.resolve(outlines);
	   })
});
```

### getItemAt(index: number)
```js
OneNote.run(function (context) {

	var page = context.application.getActivePage();
	var pageContents = page.contents;
	var firstPageContent = pageContents.getItemAt(0);
	firstPageContent.load('type');

	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function () {
			console.log("The first page content item is of type: " + firstPageContent.type);
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
