### getBase64Image()
```js

var image = null;
var imageString;

OneNote.run(function(ctx){
	// Get the current outline.			
	var outline = ctx.application.getActiveOutline();
	
	// Queue a command to load paragraphs and their types. 
	outline.load("paragraphs/type")
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
		})
		.then(function(){
			if (image != null)
			{
				imageString = image.getBase64Image();
				return ctx.sync();
			}
		})
		.then(function(){
			console.log(imageString);
		});
});
```