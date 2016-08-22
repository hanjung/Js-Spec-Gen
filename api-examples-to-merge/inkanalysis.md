### Getter

**paragraphs**
```js
OneNote.run(function (ctx) {		
	var app = ctx.application;
	
	// Gets the active page.
	var page = app.getActivePage();
	
	// Load ink paragraphs.
	page.load('inkAnalysisOrNull/paragraphs');
	
	return ctx.sync()
		.then(function() {
			console.log(page.inkAnalysisOrNull.paragraphs.items.length);
		})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
}); 
```