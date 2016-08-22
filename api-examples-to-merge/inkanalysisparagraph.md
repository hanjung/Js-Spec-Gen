### Getter

**lines**
```js
OneNote.run(function (ctx) {		
	var app = ctx.application;
	
	// Gets the active page.
	var page = app.getActivePage();
	
	// Load a line of ink words.
	page.load('inkAnalysisOrNull/paragraphs/lines');
	
	return ctx.sync()
		.then(function() {
			var inkParagraphs = page.inkAnalysisOrNull.paragraphs;
			
			// Log id of each line in ink paragraphs.
			$.each(inkParagraphs.items, function(i, inkParagraph){
				var inkLines = inkParagraph.lines;
				$.each(inkLines.items, function (j, inkLine) {
					console.log(inkLine.id);
				})
			})
		})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
}); 
```