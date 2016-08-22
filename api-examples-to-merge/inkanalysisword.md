### Getter

**wordAlternates and languageId**
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
					var inkWords = inkLine.words;
					$.each(inkWords.items, function(k, inkWord) {
					
						// Log language Id of the word
						console.log(inkWord.languageId);
						
						// Log every ink analyzed words.
						$.each(inkWord.wordAlternates, function(l, word) {
							console.log(word);									
						})
					})
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