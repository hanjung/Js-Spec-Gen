### Getter

**id**
```js
OneNote.run(function(context) {

	// Gets the active page.
	var page = context.application.getActivePage();
	var contents = page.contents;
	
	// Load page contents and their types.
	page.load('contents/type');
	return context.sync()
		.then(function(){
		
			// Load every ink content.
			$.each(contents.items, function(i, content) {
				if (content.type == "Ink")
				{
					content.load('ink/id');
				}							
			})
			return context.sync();
		})
		.then(function(){
		
			// Log ID of every ink content.
			$.each(contents.items, function(i, content) {
				if (content.type == "Ink")
				{
					console.log(content.ink.id);
				}							
			})				
		});
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
}); 
```
