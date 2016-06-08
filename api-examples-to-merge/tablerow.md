### Getter
**id, cellCount, rowIndex**
```js
OneNote.run(function(ctx) {
	var app = ctx.application;
	var outline = app.getActiveOutline();
	
	// Queue a command to load outline.paragraphs and their types.
	ctx.load(outline, "paragraphs, paragraphs/type");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync().then(function () {
		var paragraphs = outline.paragraphs;
		
		// for each table, get table rows.
		for (var i = 0; i < paragraphs.items.length; i++) {
			var paragraph = paragraphs.items[i];
			if (paragraph.type == "Table") {
				var table = paragraph.table;
				
				// Queue a command to load table.rows.
				ctx.load(table, "rows");
				return ctx.sync().then(function() {
					var rows = table.rows;
					
					// for each table row, log cell count and row index.
					for (var i = 0; i < rows.items.length; i++) {
						console.log("Row " + i + " Id: " + rows.items[i].id);
						console.log("Row " + i + " Cell Count: " + rows.items[i].cellCount);
						console.log("Row " + i + " Row Index: " + rows.items[i].rowIndex);
					}
					return ctx.sync();
				});
			}
		}
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
OneNote.run(function(ctx) {
	var app = ctx.application;
	var outline = app.getActiveOutline();
	
	// Queue a command to load outline.paragraphs and their types.
	ctx.load(outline, "paragraphs, paragraphs/type");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync().then(function () {
		var paragraphs = outline.paragraphs;
		
		// for each table, get table rows.
		for (var i = 0; i < paragraphs.items.length; i++) {
			var paragraph = paragraphs.items[i];
			if (paragraph.type == "Table") {
				var table = paragraph.table;
				
				// Queue a command to load parentTable and cells of each row in the table.
				ctx.load(table, "rows/parentTable, rows/cells");
				return ctx.sync().then(function() {
					var rows = table.rows;
					
					// for each row, log parentTable and cells
					for (var i = 0; i < rows.items.length; i++) {
						console.log("Row " + i + " Parent Table Id: " + rows.items[i].parentTable.id);
						var cells = rows.items[i].cells;
						for (var j = 0 ; j < cells.items.length; j++) {
							console.log("Row " + i + " Cell " + j + " Id: " + cells.items[j].id);
						}
					}
					return ctx.sync();
				});
			}
		}
	})
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
OneNote.run(function(ctx) {
	var app = ctx.application;
	var outline = app.getActiveOutline();
	
	// Queue a command to load outline.paragraphs and their types.
	ctx.load(outline, "paragraphs, paragraphs/type");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync().then(function () {
		var paragraphs = outline.paragraphs;
		
		// for each table, get table rows.
		for (var i = 0; i < paragraphs.items.length; i++) {
			var paragraph = paragraphs.items[i];
			if (paragraph.type == "Table") {
				var table = paragraph.table;
				
				// Queue a command to load table.rows.
				ctx.load(table, "rows");
				return ctx.sync().then(function() {
					var rows = table.rows;
					
					// delete a row.
					rows.items[2].delete();
					return ctx.sync();
				});
			}
		}
	})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```

### insertRowAsSibling()
```js
OneNote.run(function(ctx) {
	var app = ctx.application;
	var outline = app.getActiveOutline();
	
	// Queue a command to load outline.paragraphs and their types.
	ctx.load(outline, "paragraphs, paragraphs/type");
	
	// Run the queued commands, and return a promise to indicate task completion.
	return ctx.sync().then(function () {
		var paragraphs = outline.paragraphs;
		
		// for each table, get table rows.
		for (var i = 0; i < paragraphs.items.length; i++) {
			var paragraph = paragraphs.items[i];
			if (paragraph.type == "Table") {
				var table = paragraph.table;
				
				// Queue a command to load table.rows.
				ctx.load(table, "rows");
				
				// Run the queued commands
				return ctx.sync().then(function() {
					var rows = table.rows;
					rows.items[1].insertRowAsSibling("Before", ["cell0", "cell1"]);
					return ctx.sync();
				});
			}
		}
	})
})
.catch(function(error) {
	console.log("Error: " + error);
	if (error instanceof OfficeExtension.Error) {
		console.log("Debug info: " + JSON.stringify(error.debugInfo));
	}
});
```
