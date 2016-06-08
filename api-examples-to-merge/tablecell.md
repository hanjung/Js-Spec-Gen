### Getter
**id, cellIndex, rowIndex**
```js
OneNote.run(function(ctx) {
	var app = ctx.application;
	var outline = app.getActiveOutline();
	
	// Queue a command to load outline.paragraphs and their types.
	ctx.load(outline, "paragraphs, paragraphs/type");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync().then(function () {
		var paragraphs = outline.paragraphs;
		
		// for each table, get a table cell at row one and column two.
		for (var i = 0; i < paragraphs.items.length; i++) {
			var paragraph = paragraphs.items[i];
			if (paragraph.type == "Table") {
				var table = paragraph.table;
				var cell = table.getCell(1 /*Row Index*/, 2 /*Column Index*/);
				
				// Queue a command to load the table cell.
				ctx.load(cell);
				ctx.sync().then(function() {
					console.log("Cell Id: " + cell.id);
					console.log("Cell Index: " + cell.cellIndex);
					console.log("Cell's Row Index: " + cell.rowIndex);
				});
			}
		}
		return ctx.sync();
	})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

**parentTable, cells**
```js
ParentTable, ParentRow, Paragraphs
OneNote.run(function(ctx) {
	var app = ctx.application;
	var outline = app.getActiveOutline();
	
	// Queue a command to load outline.paragraphs and their types.
	ctx.load(outline, "paragraphs, paragraphs/type");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync().then(function () {
		var paragraphs = outline.paragraphs;
		
		// for each table, get a table cell at row one and column two.
		for (var i = 0; i < paragraphs.items.length; i++) {
			var paragraph = paragraphs.items[i];
			if (paragraph.type == "Table") {
				var table = paragraph.table;
				var cell = table.getCell(1 /*Row Index*/, 2 /*Column Index*/);
				
				// Queue a command to load parentTable, parentRow and paragraphs of the table cell.
				ctx.load(cell, "parentTable, parentRow, paragraphs");
				
				ctx.sync().then(function() {
					console.log("Parent Table Id: " + cell.parentTable.id);
					console.log("Parent Row Id: " + cell.parentRow.id);
					var paragraphs = cell.paragraphs;
					
					for (var i = 0; i < paragraphs.items.length; i++) {
						console.log("Paragraph Id: " + paragraphs.items[i].id);
					}
				});
			}
		}
		return ctx.sync();
	})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

### appendHtml(html: string)
```js
OneNote.run(function(ctx) {
	var app = ctx.application;
	var outline = app.getActiveOutline();
	
	// Queue a command to load outline.paragraphs and their types.
	ctx.load(outline, "paragraphs, paragraphs/type");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync().then(function () {
		var paragraphs = outline.paragraphs;
		
		// for each table, get a table cell at row one and column two and add "Hello".
		for (var i = 0; i < paragraphs.items.length; i++) {
			var paragraph = paragraphs.items[i];
			if (paragraph.type == "Table") {
				var table = paragraph.table;
				var cell = table.getCell(1 /*Row Index*/, 2 /*Column Index*/);
				cell.appendHtml("Hello");
			}
		}
		return ctx.sync();
	})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```
