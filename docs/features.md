# Features

The concept of this example is a simple user interface which makes accessing files in the Alfresco Content Services repository easy.

Often Content Management systems provide more capabilities out of the box than most users need;
providing too many capabilities to these users prevents them from working efficiently,
so they may end up using unsanctioned file management solutions which presents a proliferation of content storage
and collaboration solutions as well as compliance issues for organisations.

This application demonstrates how the complexity of Content Management can be simplified
using the Alfresco Application Development Framework to easily and quickly create custom solutions for specific user cases.

## User Interface - layout

![](images/features-01.png)

### Header (1)

The application [header](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/header) has three main elements.

Logo & app primary color - logo and color are configurable by updating the
[app.config.json](https://github.com/Alfresco/alfresco-content-app/blob/master/src/app.config.json) file in the root folder of the project,
see [How to change the logo and color](/) and [Application Configuration](/configuration) for more information

[Search](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/search) -
utilizing the [ADF Search Component](https://github.com/Alfresco/alfresco-ng2-components/tree/master/lib/content-services/search)
the app provides a 'live' search feature, where users can open files and folders directly from the Search API results.

[Current User](https://github.com/Alfresco/alfresco-content-app/tree/development/src/app/components/current-user) -
displays the user's name, and a menu where users can logout.
Optionally through updating the [app.config.json](https://github.com/Alfresco/alfresco-content-app/blob/master/src/app.config.json)
a language switching menu can be displayed.

### Side Nav (2)

The application [side navigation](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/sidenav) has two features;
a button menu and navigation links.

#### New button

The New button displays a menu which provides three actions:

- Create a new folder - provides a dialog which allows the creation of a new folder, the folder name is mandatory and the description is optional.
- Upload a file - invokes the operating system file browser and allows a user to select file(s) to upload into their current location in the content repository.
- Upload a folder - invokes the operating system folder browser and allows a user to select folder(s) to upload to their current location in the content repository.

When an upload starts the [upload component](https://github.com/Alfresco/alfresco-ng2-components/tree/master/lib/content-services/upload)
is displayed which shows the user the progress of the uploads they have started.
The upload dialog persists on the screen and can be minimised; users are able to continue using the application whilst uploads are in progress
and uploads can be canceled which will stop uploads in progress or permanently delete already completed uploads.

#### Navigation

The navigation links are configurable via the [app.config.json](https://github.com/Alfresco/alfresco-content-app/blob/master/src/app.config.json).
Default configuration creates two sections.
See [How to work with the side navigation](/) for more information about configuring the side navigation.

### Document List Layout (3)

The main area of the application is composed from a number of individual ADF components:

- [Breadcrumb](https://alfresco.github.io/adf-component-catalog/components/BreadcrumbComponent.html)
- [Toolbar](https://alfresco.github.io/adf-component-catalog/components/ToolbarComponent.html)
- [Document List](https://alfresco.github.io/adf-component-catalog/components/DocumentListComponent.html)
- [Pagination](https://alfresco.github.io/adf-component-catalog/components/PaginationComponent.html)

The application has six different Document List views which contain subtle differences depending on the content being loaded.

#### Personal Files

Personal Files retrieves all content from the logged in users home area (`/User Homes/<username>/` in the repository);
if the user is ‘admin’ who does not have a home folder then the repository root folder is shown.

Personal Files is the [Files component](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/files),
using the [Nodes API](https://api-explorer.alfresco.com/api-explorer/#/nodes).

#### File Libraries

File Libraries retrieves all the sites that the user is a member including what type of site it is; public, moderated or private.
File Libraries is the [Libraries component](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/libraries),
using the [Sites API](https://api-explorer.alfresco.com/api-explorer/#/sites).

When a user opens one of their sites then the content for that sites document library is shown.
To display the files and folders from a site (`/Sites/<siteid>/Document Library/`) the [Files component](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/files),
using the [Nodes API](https://api-explorer.alfresco.com/api-explorer/#/nodes) is used.

#### Shared Files

The Shared Files view aggregates all files that have been shared using the QuickShare feature in the content repository.
The [Shared Files component](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/shared-files)
uses the [shared-links API](https://api-explorer.alfresco.com/api-explorer/#/shared-links)
and includes extra columns to display where the file is
[located](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/location-link)
in the content repository and who created the shared link.

A feature for creating and removing Shared Links will be added in the future.

#### Recent Files

The Recent Files view shows all the files that have been modified within the last 30 days by the current user.
The [Recent Files](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/current-user)
component uses the Search API to query SOLR for changes made by the user and includes an extra column to display where the file is
[located](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/location-link)
in the content repository.

#### Favorites

The Favorites view shows all files and folders from the content repository that have been marked as a favorite by the current user.
The [Favorites](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/favorites) component uses the
[favorites](https://api-explorer.alfresco.com/api-explorer/#/favorites) API to retrieve all the favorite nodes for the user
and includes an extra column to display where the file is
[located](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/location-link)
in the content repository.

#### Trash

The Trash view shows all the items that a user has deleted, admin will see items deleted by all users.
The actions available in this view are Restore and Permanently Delete.
The [Trashcan](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/trashcan) component uses the
[trashcan](https://api-explorer.alfresco.com/api-explorer/#/trashcan) API to retrieve the deleted items
and perform the actions requested by the user and includes an extra column to display where the file is
[located](https://github.com/Alfresco/alfresco-content-app/tree/master/src/app/components/location-link)
in the content repository.

#### Actions and the Actions Toolbar

All the views incorporate the [toolbar](https://alfresco.github.io/adf-component-catalog/components/ToolbarComponent.html)
component from the Alfresco Application Development Framework;
apart from the Trash view they all display the following actions when the current user has the necessary permissions,
actions are automatically hidden when the user does not have permission.

<table>
<thead>
    <th>Action</th>
    <th>File</th>
    <th>Folder</th>
</thead>
<tbody>
    <tr>
        <td>View</td>
        <td>
            Opens the selected file using the [Preview](https://github.com/Alfresco/alfresco-content-app/tree/development/src/app/components/preview) component,
            where the file cannot be displayed natively in a browser a PDF rendition is obtained from the repository.
        </td>
        <td>Not applicable</td>
    </tr>
    <tr>
        <td>Download</td>
        <td>Downloads single files to the users computer, when multiple files are selected they are compressed into a ZIP and then downloaded.</td>
        <td>Folders are automatically compressed into a ZIP and then downloaded to the users computer.</td>
    </tr>
    <tr>
        <td>Edit</td>
        <td>Not applicable</td>
        <td>The folder name and description can be edited in a dialog.</td>
    </tr>
    <tr>
        <td>Favorite</td>
        <td colspan="2">
            Toggle the favorite mark on or off for files and folders, when multiple items are selected
            and one or more are not favorites then the mark will be toggled on.
        </td>
    </tr>
    <tr>
        <td>Copy</td>
        <td colspan="2">
            Files and folders can be copied into other locations in the content repository using the
            [content-node-selector](https://alfresco.github.io/adf-component-catalog/components/ContentNodeSelectorComponent.html) component;
            once the copy action has completed the user is notified and can undo the action (which permanently deletes the copies created).
        </td>
    </tr>
    <tr>
        <td>Move</td>
        <td colspan="2">
            Files and folders can be moved into other locations in the content repository using the
            [content-node-selector](https://alfresco.github.io/adf-component-catalog/components/ContentNodeSelectorComponent.html) component;
            once the move action has completed the user is notified and can undo the action (which moves the items back to the original location).
        </td>
    </tr>
    <tr>
        <td>Delete</td>
        <td colspan="2">
            Files and folders can be deleted from their location in the content repository;
            once the delete action has completed the user is notified and undo the action (which restores the items from the trash).
        </td>
    </tr>
</tbody>
</table>

As well as the actions available in the toolbar users can single click an item to select it,
or double click on a file to view it, and a folder to open it.