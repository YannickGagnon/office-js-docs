# OneNote add-ins overview (Preview)

<!-- Introduction--for an example, see [Word add-ins overview](https://dev.office.com/docs/add-ins/word/word-add-ins-programming-overview).

- Describe common scenarios.
- Describe what add-ins can do.
- Include an image of an add-in that illustrates scenario/best practices.
- Specify target platforms.

-->

You can create task pane add-ins, content add-ins, and add-in commands that interact with OneNote objects and connect to web services or other web-based resources. Currently, OneNote add-ins are only supported for OneNote Online. 


##  JavaScript APIs for OneNote

<!-- Introduce the APIs used to develop add-ins, including client-specific APIs and Office.js. Explain the scenarios in which to use them. Link to relevant reference documentation.

-->

You can use two sets of JavaScript APIs to interact with the objects and metadata in a OneNote notebook. 

- The [OneNote JavaScript API](../../reference/onenote/onenote-add-ins-javascript-reference.md). 
 This is a strongly-typed object model that you can use to create OneNote add-ins for OneNote Online. This API uses promises, and provides access to OneNote-specific objects like [sections](../../reference/onenote/section.md), [pages](../../reference/onenote/page.md), and [paragraphs](../../reference/onenote/paragraph.md). 

- The [JavaScript API for Office](https://dev.office.com/reference/add-ins/javascript-api-for-office?product=word), which was introduced in Office 2013. 
 This is a shared API -- many of the objects can be used in add-ins hosted by two or more Office clients. This API uses callbacks extensively. 

We recommend that you use the OneNote JavaScript API because it better aligns with OneNote functionality and the object model is easier to use. For example, the following code adds a page to the current section.

```js
OneNote.run(function (context) {
    var page = context.application.getActiveSection().addPage("My new page");        
    page.load('id,title');
    return context.sync()
        .then(function () {
            console.log("Page name: " + page.title);
            console.log("Page ID: " + page.id);
        });
})
```

Currently, OneNote Online add-ins support the OneNote JavaScript API and some of the shared API. For details, see the [OneNote API reference documentation](../../reference/onenote/onenote-add-ins-javascript-reference.md).


## Next steps

Ready to create your first OneNote add-in? See [Build your first OneNote add-in (Preview)](onenote-add-ins-getting-started.md). Use the [add-in manifest](../overview/add-in-manifests.md) to describe where your add-in is hosted and how it is displayed, and define permissions and other information.

To learn more about how to design a world class OneNote add-in that creates a compelling experience for your users, see [Design guidelines](../design/add-in-design.md) and [Best practices](../design/add-in-development-best-practices.md).

<!-- Does this apply to OneNote yet? 
After you develop your add-in, you can [publish](../publish/publish.md) it to a network share, to an app catalog, or to the Office Store.
-->

## What's next for OneNote add-ins

<!-- Describe and link to APIs available on Open Spec page. Link to change log if applicable. Provide a roadmap for new APIs and features.

-->

## Additional resources

- [Office Add-ins platform overview](../overview/office-add-ins.md)
- [OneNote JavaScript API reference (Preview)](../../reference/onenote/onenote-add-ins-javascript-reference.md)
