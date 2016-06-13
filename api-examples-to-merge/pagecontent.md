### select()
```js
OneNote.run(function (context) {

	var page = context.application.getActivePage();
	var pageContents = page.contents;
	pageContents.load('type');

	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function () {
			var firstPageContent = pageContents.getItemAt(0);
			firstPageContent.select();
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

### delete()
```js
OneNote.run(function (context) {

	var page = context.application.getActivePage();
	var pageContents = page.contents;

	var firstPageContent = pageContents.getItemAt(0);
	firstPageContent.load('type');

	// Run the queued commands, and return a promise to indicate task completion.
	return context.sync()
		.then(function () {
			if(firstPageContent.isNull === false) {
				firstPageContent.delete();
				return context.sync();
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