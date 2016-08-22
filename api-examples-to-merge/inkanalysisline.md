### Getter

**words**
```js
OneNote.run(function (ctx) {		
	var app = ctx.application;
	
	// Gets the active page.
	var page = app.getActivePage();
	page.load('inkAnalysisOrNull/paragraphs/lines/words');
	
	return ctx.sync()
		.then(function() {
			var inkParagraphs = page.inkAnalysisOrNull.paragraphs;
			$.each(inkParagraphs.items, function(i, inkParagraph) {
				var inkLines = inkParagraph.lines;
				$.each(inkLines.items, function(j, inkLine) {
					// Word counts in a line.
					console.log(inkLine.words.items.length);
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