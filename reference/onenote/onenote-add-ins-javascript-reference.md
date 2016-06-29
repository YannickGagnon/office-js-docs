# OneNote JavaScript API reference (Preview)

*Applies to: OneNote Online*

OneNote provides a rich set of APIs that you can use to create add-ins for OneNote Online. Use these APIs to create page content, interact with OneNote objects, and connect to web services or other web-based resources.

You can use two JavaScript APIs to interact with the objects and metadata in a OneNote notebook:
- OneNote JavaScript API - Introduced in Office 2016.
- JavaScript API for Office (Office.js) - Introduced in Office 2013.

The APIs are accessed through Office.js. During this preview period, reference it from the following location:

- https://richapiaddin.azurewebsites.net/App/Office/Office.js


## OneNote JavaScript API

The OneNote JavaScript API is loaded by Office.js. It is the primary API for OneNote add-ins and is accessed through the [Application](application.md) object.

Rather than providing individual asynchronous APIs for retrieving and updating each of these objects, the OneNote JavaScript API provides "proxy" JavaScript objects that correspond to the real objects running in OneNote, such as [Notebook](notebook.md), [Section](section.md), and [Page](page.md). 

You can interact with these proxy objects by synchronously reading and writing their properties and calling synchronous methods to perform operations on them. These interactions with proxy objects aren't immediately realized in the running script. 
 The **context.sync** method synchronizes the state between your running JavaScript and the real objects by executing queued instructions and retrieving properties of loaded OneNote objects for use in your script.

The basic flow goes something like this: 

1- Get the application instance from the context.

2- Create a proxy that represents the OneNote object you want to work with. You interact synchronously with proxy objects by reading and writing their properties and calling their methods. 

3- Call **load** on the proxy to fill it with the property values specified in the parameter. This call is added to the queue of commands. 

   Method calls to the API (such as `context.application.getActiveSection().pages;`) are also added to the queue.
    
4- Call **context.sync** to run all queued commands in the order that they were queued. This synchronizes the state between your running script and the real objects, and by retrieving properties of loaded OneNote objects for use in your script. You can use the returned promise object for chaining additional actions.

For example: 

```
    function getPagesInSection() {
        OneNote.run(function (context) {
            
            // Get the pages in the current section.
            var pages = context.application.getActiveSection().pages;
            
            // Queue a command to load the id and title for each page.            
            pages.load('id,title');
            
            // Run the queued commands, and return a promise to indicate task completion.
            return context.sync()
                .then(function () {
                    
                    // Read the id and title of each page. 
                    $.each(pages.items, function(index, page) {
                        var pageId = page.id;
                        var pageTitle = page.title;
                        console.log(pageTitle + ': ' + pageId); 
                    });
                })
                .catch(function (error) {
                    app.showNotification("Error: " + error);
                    console.log("Error: " + error);
                    if (error instanceof OfficeExtension.Error) {
                        console.log("Debug info: " + JSON.stringify(error.debugInfo));
                    }
                });
        });
    }
```

### OneNote object model diagram 

The following diagram represents the top-level objects in the OneNote JavaScript API.

  ![OneNote object model diagram](../../images/onenote-om.png)
  
  
## JavaScript API for Office

The [JavaScript API for Office](../javascript-api-for-office.md) is shared across Office applications and is accessed through the [Document](../shared/document.md) object. 
OneNote add-ins support only the following members of the JavaScript API for Office:

- [Office.context.document.getSelectedDataAsync](../shared/document.getselecteddataasync.md). **Office.CoercionType.Text** and **Office.CoercionType.Matrix** only.
- [Office.context.document.setSelectedDataAsync](../shared/document.setselecteddataasync.md). **Office.CoercionType.Text**, **Office.CoercionType.Image**, and **Office.CoercionType.Html** only. 
- [Office.context.document.settings.get](../shared/settings.get.md). Settings are supported by content add-ins only.
- [Office.context.document.settings.set](../shared/settings.set.md). Settings are supported by content add-ins only.
- [Office.EventType.DocumentSelectionChanged](../shared/document.selectionchanged.event.md)

>In general, you use this API to do something that isn't supported in the OneNote JavaScript API or for development across Office applications.


## *Additional section(s) to include programming concepts/examples*

<!-- Optional section to provide specifics and examples for developing with the API.

-->

## Open OneNote API specifications

<!-- Optional. Link to the [Open API specifications](../../reference/openspec.md) page for details about new APIs in development.

-->

## Additional resources

- [Office Add-ins platform overview](../../docs/overview/office-add-ins.md)
- [OneNote JavaScript API programming overview (Preview)](../../docs/onenote/onenote-add-ins-programming-overview.md)
- [Build your first OneNote add-in (Preview)](../../docs/onenote/onenote-add-ins-getting-started.md)
- [OneNote add-in samples on GitHub](https://github.com/OfficeDev?utf8=%E2%9C%93&query=onenote)
