# The YouTrack's Advanced Search and Data Extrapolation #   

A summary of the **YouTrack**'s official documentation.  
*Made by Adriano Rueda & Nicola Pucci*

## Index
- [Advanced Search](#advanced_search)  
    1. [Overview](#overview_1)  
    2. [Definition](#definition)  
    3. [How to activate it](#how_to_activate_it)  
    4. [Syntax](#syntax)  
    5. [Search Suggestions](#search_suggestions)  
    6. [Wildcards in Attribute-based Search](#wildcards)  
    7. [Single Value Search](#single_value_search)  
    8. [Searching for a Range of values](#searching_for_a_range_of_values)  
    9. [Searching in Attributes that Store Text](#searching_in_attributes_that_store_text)  
    10. [Query Completion](#query_completion)   
    11. [Visual Indicators](#visual_indicators)
  
- [REST API & Apps](#data_extrapolation)  
    1. [Overview](#overview_2)  
    2. [Authentication](#authentication)
    3. [Request Parametres](#parameters)
    4. [Fields](#fields)  
    5. [Custom Fields](#custom_fields)  
    6. [Query](#query)  
    7. [Pagination](#pagination)  
    8. [Data obtained via URL](#data)  
    9. [Apps](#apps)  
        - [Rules](#rules)  
        - [HTTP Handler](#http-handler)
    10. [Entity Attributes](#entity_attributes)  
        - [User](#user)  
        - [Project](#project)  
        - [Issue](#issue)

## <a id='advanced_search'></a> Advanced Search ##
*[Official documentation](https://www.jetbrains.com/help/youtrack/server/attribute-based-search.html)*

### <a id='overview_1'></a> 1. Overview ###
What is advanced search and how does it differ from simple search?

### <a id='definition'></a> 2. Definition ###
The YouTrack's Advanced search is the second option for searching issues in the platform. It allows you to search issues using attribute and value pairs.  

### <a id='how_to_activate_it'></a> 3. How to activate it  
To activate advanced search you have to select it in the settings button ![alt text](image.png) to the right of the search box. 

### <a id='syntax'></a> 4. Syntax ###
To compose an attribute-based search query there are various ways. Below that with examples:
- ` : ` to specify the value of an attribute

- ` # ` to find issues with that value or state

- ` - ` to exclude a value

- ` .. ` to find issues between two values

- ` {} ` to enclose values that contain spaces with braces

- ` and ` to include one or more issues to the original one 

- ` or ` to include an alternative issue 

### <a id='search_suggestions'></a> 5. Search suggestions
Search suggestion can be made Smart o for User Accounts.  

Smart:  
The values suggested for subsequent attributes are restricted to the sets of values used in these projects are well when you search for a value only used in a specific set of projects. Un example occurs when you search for items that are assigned the `State: On hold`.

User Accounts:  
Search attributes that reference users display usernames in the auto-completion list.   These suggestions are only offered when you are granted the Read User Basic permissions in at least one project.

### <a id='wildcards'></a> 6. Wildcards in Attribute-based Search
You can use the `*` wildcards to find values in custom fields that store an enum, a state, an ownedField, a version, a build or a string. You can also use that for sprint names, project names and tags. Wildcards are not supported for fields that store a `user`, a `date`, a `period`, a `float` or an `integer`.  
The `?` wildcard can be used only in text-based searches. It isn’t supported in attribute-based search queries, even for custom fields that store values of type `string`.


### <a id='single_value_search'></a> 7. Single Value Search 
YouTrack allows to search for single values replacing a number sign or a minus in front of the value to put.  
Some example:
- **#{In Progress} -{Support Request} :**  
finds all issues that are in progress that are not support requests;

- **Pricing: {-100} .. {-500} :**  
finds issues with pricing values between -100 and -500;

- **#me -Resolved :**  
finds all issues that are all except resolved.

### <a id='searching_for_a_range_of_values'></a> 8. Searching for a Range of Values
Like YouTrack allows you to search for single values allows you also to search for a Range of Values.  
To search for a range:  
1. Enter the name of the attribute followed by a `:`;
2. Enter the value that defines the upper or lower bound of the range followed by `..`;
3. Enter the value that closes the range. use the `*` as a wildcard.
4. Execute the search query.  

Some Query of Search for a Range of Values with 'closed' like Range:
1. **Spent time: 1h .. 2h :**  
returns issues in wich amount of spent time is at least one hour but not greater than two hours.

2. **Created: 2018-03-10 .. 2018-03-13 :** 	
returns issues created from March 10 to March 13, 2018. 

3. **Actual start: 2025-03-25T09:00 .. 2025-03-25T18:00 :**  
returns issues started on March 25, 2025 between 09:00 and 18:00.

4. **Story Points: 1 .. 5 :**  
returns issues that are assigned at least one story point but not more than five story points.

5. **Price: {-100} .. {-500} :**  
returns issues that have a price value between -100 and -500.

6. **Type: Bug #Resolved Fixed in builds: 41238 .. 43006 :**  
returns resolved issues that are categorized as bugs that are fixed in builds from 41238 to 43006.  

Now with 'open' like Range:  

1. **Spent time: 8h1m .. * :**  
returns issues in which the amount of spent time is greater than one day.

2. **Created:*.. 2018-03-13 :**  
returns issues created before March 10, 2018.

3. **Actual start: 2025-03-25T014:00 .. * :**  
returns issues started on and after March 25, 2025 at 14:00.

4. **Story Points: 10 .. * :**  
returns issues that are assigned 10 story points and higher.

5. **Price: {-500} .. * :**  
returns issues with price value of -500 or higher.

6. **votes: 30 .. * #Unresolved :**  
returns unresolved issues that have 30 or more votes.

7. **Priority: * .. Major #Unresolved :**  
returns unresolved issues that are assigned major priority or higher.

8. **Type: Bug #Resolved Fixed in builds: 41238 .. * :**  
returns resolved issues that are categorized as bugs that are fixed in builds 41238 and higher.

### <a id='searching_in_attributes_that_store_text'></a> 9. Searching in Attributes that Store Text ###
Attributes like the issue summary, description or comments are stored as text. When you specify one of these attributes in your search query YouTrack understands that you want to check the specified attribute for one or more words. Consequentially YouTrack searches inside the attribute for matches to the specified text. The syntax between text search and attribute-based search is slightly different. In this kind of search you can enter as many words as you want, in any order.  See it with some queries:

1. **project: IDEADEV commenter: John Davis description: customer support**  
returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description.

2. **project: IDEADEV commenter: John Davis description: "customer support"**  
returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description as a phrase.

### <a id='query_completion'></a> 10. Query Completion
The search box supports query completion. This feature suggests possible attributes and values that are based on your current input. When you start to enter in an empty search query YouTrack suggests attributes and values for keywords that match with the current input. You can select that keyword from the list using your mouse or keyboard.
There are two modes of operation:  
1. If you select an attribute the query completion suggests possible values for the selected attribute.
2. If you select a single value that value is added to the query.

### <a id='visual_indicators'></a> 11. Visual Indicators
When you enter a query in the search box YouTrack applies visual indicators to the query. This formatting tells you that your query has been parsed as a structured search.  
Be careful to some warnings:
1. Issue attributes are shown as normal text without formatting.
2. Attribute values are shown in blue text.
3. Words or phrases that are parsed as a text search are shown in gold.
4. Invalid parameters are underlined in red.

## <a id='data_extrapolation'></a> 2. REST API & Apps
*[Official documentation](https://www.jetbrains.com/help/youtrack/devportal/youtrack-rest-api.html)*

**<a id='overview_2'></a>1. Overview**

YouTrack exposes a comprehensive REST API that allows administrators, developers, and integrations to access, query, and modify YouTrack data programmatically.  
The API supports read and write operations on issues, projects, users, attachments, workflows, custom fields, and system configuration.

All API endpoints follow a consistent structure:
    <youtrack-base-url>/api/<entity>?fields=<requestedFields>&<filters>

Requests can be authenticated using permanent tokens or OAuth 2.0.

------------------------------------------------------------
### <a id='authentication'></a> 2. Authentication

YouTrack supports two primary authentication mechanisms:

A. Permanent Tokens (recommended)  
Created from the user’s profile.  
Add them to requests as:
    Authorization: Bearer <token>

B. OAuth 2.0  
Supports multi-application workflows and delegated permissions.

All write operations (POST/PUT/DELETE) must include:
    Content-Type: application/json

Example request header:
    GET <youtrack>/api/issues?fields=id,summary
    Authorization: Bearer <token>
    Accept: application/json

------------------------------------------------------------


### <a id='parameters'></a> 3. Request Parameters

YouTrack REST API accept some parameters:
- *fields*
- *query*
- *$top*
- *skip*

#### <a id='fields'></a> 4. Fields  
The API does not return all fields by default.  
Only key identifiers such as 'id' and '$type' are always included.  

To retrieve additional data, the `fields=` parameters must explicitly list the attributes to return.  

Example:
```
https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)
```
the server will return something like:

```
[
    {
        'project':{
            'name':'Sample Project,
            '$type':'Project'
        },
            'summary':'1st issue of Sample Project',
            'id':'2-150',
            '$type':'Issue'
    },{
        'project':{
            'name':'Demo Project,
            '$type':'Project'
        },
            'summary':'1st issue of Demo Project',
            'id':'2- 120',
            '$type':'Issue'
    },
]
```
---

##### <a id='custom_fields'></a> 5. Custom Fields
*CustomField* is an entity that defines a common custom field, wich contains basic attributs like `name` and `fieldType`.
You must explicitly declare the customFields' fields you want in your response.

```
{your_url}/api/issues/?fields=id,summary,customFields(id,projectCustomField(field(name)),value(name))
```

---

#### <a id='query'></a> 6. Query
***You can only use queries in `GET` requests that work with collections of issues or tags***  

`query=` allows you to filter by the value of the attribute.  
The syntax for the queries is the same as *[Advanced Search](#advanced_search)*.  

Example:
```
https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=project:+%7BSample+Project%7D

```
The response will look like this:

```
[
    {
        'project':{
            'name':'Sample Project,
            '$type':'Project'
        },
            'summary':'1st issue of Sample Project',
            'id':'2-150',
            '$type':'Issue'
    }    
]
```
---

Here is a bit more complex query.
we are looking for issues tassigned to 'john.doe', not yet resolved, and contain the
word 'summary'.
```
https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=for:+john.doe+%23Unresolved+summary
```
---
#### <a id='pagination'></a> 7. Pagination
We cannot extract a large amount of data in a single request.
Instead, we can use pagination to retrieve large datasets across multiple requests. By default, YouTrack returns around 40 entities (depending on the entity type).
By specifying the $top and $skip parameters, we can control how many entities to retrieve and how many to skip.
**example**:
```
{base_url}/api/issues/?fields=id,summary&$top=40&$skip=80
```
the example above will skip the first 80 results and return results between 81 and 120 (if they exist)

---

#### <a id='data'></a> 8. Data obtained via URL
To retrieve data via URL:  
https://{HOST:PORT}{url}

Example(retrieve attachments): 
We need the field `url` of the attachments.  


```
/api/issues/{issue_id}/attachments
```

the response will look like this:
```
[
    {
        'charset':null,
        'extension':'png',
        'url':'/youtrack/api/files/7-2?sign=MTYxMzE2OTAwMDAwMHwxLTF8Ny0yfHdrRzJCYzA2cnZEclFLUzBLWEp6NXZGNE5kczA5eGVIVUUw%0D%0AbF9BVHZ3V3MNCg%0D%0A&updated=1569436739288",
        'mimeType':'image/png',
        'name':'attachment-name.png',
        'size':219293,
        '$type':'IssueAttachment'
    }
]
```

now we can use the `url` in the attachment to receive the attachment.
```
https://{HOST:PORT}{url_in_attachments}
```
**Note:**  
we don't need to include the token in the header of the request to retrieve the attachments because the client is identified using the `sign=` parameter in the `url` received from `/api/issues/{issue_id}/attachments`.  

If we want to take all the attachments from the issues of the Project MioProject:
```
{base_url}/api/issues?fields=id,attachments(url)&query=project:MioProject
```
This will return a list of issues, each issue will have id and a list of attachments and each attachment will have his url
```json
{
	[
    {
      "id": "DEMO-1",
      "attachments":[
          {
              "charset":null,
              "extension":"png",
              "url":"/youtrack/api/files/7-2?sign=MTYxMzE2OTAwMDAwMHwxLTF8Ny0yfHdrRzJCYzA2cnZEclFLUzBLWEp6NXZGNE5kczA5eGVIVUUw%0D%0AbF9BVHZ3V3MNCg%0D%0A&updated=1569436739288",
              "mimeType':'image/png",
              "name":"attachment-name.png",
              "size":219293,
              "$type":"IssueAttachment"
          }
      ]
    },    {
      "id": "DEMO-2",
      "attachments":[
          {
              "charset":null,
              "extension":"png",
              "url":"/youtrack/api/files/7-5?sign=MTYxMzE2OTAwMDAwMHwxLTF8Ny0yfHdrRzJCYzA2cnZEclFLUzBLWEp6NXZGNE5kczA5eGVIVUUw%0D%0AbF9BVHZ3V3MNCg%0D%0A&updated=1569436739288",
              "mimeType':'image/png",
              "name":"attachment-name.png",
              "size":219293,
              "$type":"IssueAttachment"
          }
      ]
    }
  ]
}
```
---

### <a id='apps'></a> 9. Apps
Check [app quick start guide](https://www.jetbrains.com/help/youtrack/devportal/apps-quick-start-guide.html) for more.  

`Apps` in Youtrack allow you to customize your youtrack.
One of the customizations you can do is implementing modules,  
modules can be mainly `rules` or `HTTP Handlers`.

**Could not confirm with Official documentation**  
It seems that script have a limit on execution time and memory usage.
(According to some LLM we are talking abt 30-60 sec execution time and in terms of issues about 2000)


To start creating an app run:
```bash
npm build @jetbrains/youtrack-app@latest
```
Follow the prompts provided by the generator.

now you can customize your app, remember that modules are `js` files.

When your app is ready, run:
```bash
npm run build
```
then
```bash
npm run upload -- --host <Your_Url> --token <You_Token>
```
If you rather add it directly on YouTrack, go to `app`, in the top-right corner there will be an `add app` button, press it then press `Upload .zip`

### <a id='modules'></a> Rules
In YouTrack, you can define **rules** to execute actions, validate data, or perform other operations based on specific patterns and conditions.
Rules are applied at the project level.

The main attributes of the rules are:
- **guard** - specifies the conditions under which the rule is triggered.
- **requirement** - defines the prerequisites needed for the rule to run.
- **action** - describes what the rule is supposed to do.


There are 4 types of `rules`:

- **onChange** – triggered whenever a field is updated *(available only for Issues and Articles)*.

- **onSchedule** – triggered according to a defined schedule.

- **stateMachine** – controls transitions between values of a custom field *(available only for enumerated custom fields)*.

- **action** – can be executed via a command or from the Show more menu of an entity *(available for Issues, Articles, Issue comments, Article comments, Issue attachments, and Article attachments)*.

Here's an example of an `onChange` rule:
```js
const entities = require('@jetbrains/youtrack-scripting-api/entities');
const workflow = require('@jetbrains/youtrack-scripting-api/workflow');

exports.rule = entities.Issue.onChange({
  title: 'title of the rule',
  guard: (ctx) => {//this will define if the action should be played or not, it returns a boolean
    return !ctx.issue.fields.Assignee && ctx.issue.fields.Subsystem;
  },
  action: (ctx) => {//what to do if the conditions are met
    const issue = ctx.issue;
    const fs = issue.fields;
    if ((issue.isReported && (fs.isChanged(ctx.Subsystem) || issue.isChanged('project'))) || issue.becomesReported) {
      if (fs.Subsystem.owner) {
        if (ctx.Assignee.values.has(fs.Subsystem.owner))
          fs.Assignee = fs.Subsystem.owner;
        else
          workflow.message(
              "{0} is set as the owner of the {1} subsystem but isn't included in the list of assignees for issues in this project. " +
              "The workflow that automatically assigns issues to the subsystem owner cannot apply this change.",
              fs.Subsystem.owner.fullName, fs.Subsystem.name);
      }
    }
  },
  requirements: {//requirements needed for the action
    Assignee: {
      type: entities.User.fieldType
    },
    Subsystem: {
      type: entities.OwnedField.fieldType
    }
  }
});
```

*The rule above is triggered whenever an issue without an Assignee is updated and has a **Subsystem**.
The rule attempts to assign the issue to the owner of that Subsystem.*

---

### <a id='http-handler'></a>HTTP Handlers

A HTTP Handler is a type of [custom module](https://www.jetbrains.com/help/youtrack/server/custom-scripts.html)

Your HTTP Handler can access data data through the YouTrack Web API or the internal YouTrack APIs.

Here's an example of a HTTP Handler

```js
const search = require('@jetbrains/youtrack-scripting-api/search'); // Workflow Api have to be imported
const entities = require('@jetbrains/youtrack-scripting-api/entities');

/*
This handler is reachable by:
{server_url}/api/extensionEndpoints/{app_name}/{module_name}/{endpoint_path}
*/
exports.httpHandler = {
  endpoints: [
    {
      /*scope: 'issue' you can specify the scope of the handler*/
      method: 'GET', 
      path: 'debug',
      /*permissions: ['CAN_READ_ISSUES'] you can set the permissions needed for the endpoint(Array of String)*/
      handle: function handle(ctx) {
        const issues = search.search(
          null,//Watch Folder, if none scope is global
          {
            query:'issue id: DEMO-10' //same syntax as Advanced Search in GUI
            
          },
          null//user account that executes the search query, if none is provided,then uses the owner of the token used for the request
        )
        
		const results = issues.map(issue => {
          return {
            id: issue.id,
            idReadable: issue.idReadable,
            summary: issue.summary,
            state: issue.State ? issue.State.name : 'N/A',
            reporter: issue.reporter.fullName 
          };
        });
        ctx.response.json(results);
      }
    },
    {
      method	:'POST',
      path:"debug",
      handle: function handle(ctx) {
        const body = ctx.request.json()
        const id = body.id? body.id : 'DEMO-1'
        
        const issue = entities.Issue.findById(id)
        
        const results = {
          id:issue.id,
          summary:issue.summary
        }
        
        ctx.response.json(results)
      }
    }
  ]
};
```

---

### <a id='entity_attributes'></a> 10. Entity Attributes
*Below we list the three main entities. For a complete overview of all available entities, refer to the official [documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entities.html).*

*(Each one can be divided into required and optional attributes. All of them are represented by a **Field** and a **Type**, plus a description if available.)*

## User 
*[click here](https://www.jetbrains.com/help/youtrack/devportal/api-entity-User.html#x7x140_6) to see his official documentation*

- **id** String  
- **fullName** String  
- **tags** Array of [Tags](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Tag.html)  
- **savedQueries** Array of [savedQueries](https://www.jetbrains.com/help/youtrack/devportal/api-entity-SavedQuery.html) 
- **login** String 
- **email** String. Can be null
- **ringId** String. Usable for operations in Hub and for matching users between YouTrack and the first one. Can be null
- **guest** Boolean 
- **online** Boolean 
- **banned** Boolean 
- **avatarUrl** String 
- **profiles** [UserProfiles](https://www.jetbrains.com/help/youtrack/devportal/api-entity-UserProfiles.html) 

## Project
*[click here](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Project.html) to see his official documentation*

- **id** String  

- **archived** Boolean  

- **createdBy** [User](#user)  

- **customFields** [ProjectCustomField](https://www.jetbrains.com/help/youtrack/devportal/api-entity-ProjectCustomField.html)   

- **description** String  

- **fromEmail** String  

- **iconUrl** String  

- **issues** Array of [Issues](#issue)  

- **leader** [User](#user)  

- **name** String  

- **replyToEmail** String   

- **shortName** String  

- **startingNumber** Long  

- **team** [projectTeam](https://www.jetbrains.com/help/youtrack/devportal/api-entity-ProjectTeam.html)  

- **template** Boolean


## Issue
*[click here](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Issue.html#-i6gy7u_4) to see his official documentation*

- **id** String

- **attachments** Array of [IssueAttachments](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueAttachment.html)

- **comments** Array of [IssueComments](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueComment.html)

- **commentsCount** Int

- **created** Long 

- **customFields** Array of [IssueCustomFields](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueCustomField.html)

- **description** String. Can be null

- **draftOwner** [User](#user). Shows the creator of the draft if the issue is a draft. Shows null if the issue is reported

- **externalIssue** [ExternalIssue](https://www.jetbrains.com/help/youtrack/devportal/api-entity-ExternalIssue.html). Can be null

- **idReadable** String

- **isDraft** Boolean

- **links** Array of [IssueLinks](https://www.jetbrains.com/help/youtrack/devportal/resource-api-issues-issueID-links.html)

- **numberInProject** Long

- **parent** [IssueLinks](https://www.jetbrains.com/help/youtrack/devportal/resource-api-issues-issueID-links.html). Can be null

- **pinnedComments** Array of [IssueComments](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueComment.html)

- **project** [Project](#project). Can be null

- **reporter** [User](#user). Can be null

- **resolved** Long. *time in milliseconds* Can be null

- **subtasks** [IssueLink](https://www.jetbrains.com/help/youtrack/devportal/resource-api-issues-issueID-links.html)

- **summary** String

- **tags** Array of [Tags](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Tag.html). Can be null

- **updated** Long

- **updater** [User](#user). Can be null

- **visibility** [Visibility](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Visibility.html). Can be null

- **voters** [IssueVoters](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueVoters.html)

- **votes** Int

- **watchers** [IssueWatchers](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueWatchers.html)

- **wikifiedDescription** String
