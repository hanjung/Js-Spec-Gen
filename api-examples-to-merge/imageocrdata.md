### Getter
**ocrText and ocrLanguageId**
```js
var image = null;

OneNote.run(function(ctx){
	// Get the current outline.
	var outline = ctx.application.getActiveOutline();

	// Queue a command to load paragraphs and their types.
	outline.load("paragraphs")
	return ctx.sync().
		then(function(){
			for (var i=0; i < outline.paragraphs.items.length; i++)
			{
				var paragraph = outline.paragraphs.items[i];
				if (paragraph.type == "Image")
				{
					image = paragraph.image;
				}
			}
			if (image != null)
			{
			   image.load("ocrData");
			}
			return ctx.sync();
		})
		.then(function(){
			
			// Log ocrText and ocrLanguageId
			console.log(image.ocrData.ocrText);
			console.log(image.ocrData.ocrLanguageId);
		});
}).catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```
