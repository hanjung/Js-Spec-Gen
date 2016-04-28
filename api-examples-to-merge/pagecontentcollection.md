### Getter

**items**
```js
OneNote.run(function (context) {

    // Get the collection of pageContent items from the page.
    var pageContents = context.application.activePage.getContents();

    // Queue a command to load the type of each pageContent.
    pageContents.load("type");

    // Run the queued commands, and return a promise to indicate task completion.
    return context.sync()
        .then(function () {

            $.each(pageContents.items, function(index, pageContent) {
                console.log("PageContent type: " + pageContent.type);
            });
        });
    })                
    .catch(function(error) {
        console.log("Error: " + error);
        if (error instanceof OfficeExtension.Error) {
            console.log("Debug info: " + JSON.stringify(error.debugInfo));
        }
    });
```