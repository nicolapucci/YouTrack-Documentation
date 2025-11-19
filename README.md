### YouTrack REST Api

#### notes
- header `Authorization: 'Bearer {your_token}'` is required for every request

- use **Accept** HTTP header to indicate the expected response data: `application/json`

- enpoint are exposed at: `{your_youtrack_url}/api`

- for POST and PUT use **Content-Type** `application/json` 

- YouTrack supports `OAuth2.0` but prefers the usage of a [permanent token](#permanemt-token)

- eats and returns json data

- you can add custom endpoints


### Fields and Query Syntax

in YouTrack REST Api, when you sent a request to a resource, by default the servers sends back only the database ID and the `$type` of the resource entity.
To receive attributes in response from the server you must explicitly specify the `fields` parameter.


#### Fields
`fields` allows you to choose the attributes you get the response.  
Here's an example where we only want `id`, `summary` and `project name` of the issue:
```
https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)
```
the server will seturn something like:

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

### Custom Fields
`CustomField` is an entity that defines a common custom field wich contains basic attributs like `name` and `fieldType`.

If you want to see the custom fields of the issues you have to include `customFields` in the `fields`, you also have to specify which of the customField's attribute you want in your response

```
{your_url}/api/issues/?fields=id,summary,customFields(id,projectCustomField(field(name)),value(name))
```

---

### Query
***You can only use queries in `GET` requests that work with collections of issues or tags***  

`query` allows you to filter by the value of the attribute.  
Here's an example where we want to see the issues of the project `Sample Project`.
```
https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=project:+%7BSample+Project%7D

```
the response body will contain data only for the matching issues

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
we are looking for issues that are assigned to the user 'john.doe', not yet resolver, and contain the
word 'summary'.
```
https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=for:+john.doe+%23Unresolved+summary
```
### Attachments
To get the attachments of a specific issue we need the attachment's url.  
To get the urls we'll take the list of attachments of the issue

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
    },
    .
    .
    .
    .
    .
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





## Entity Attributes

### <a id='user'></a>User

### <a id='project'></a>Project

### <a id='issue'></a>Issue
*[see official Issue documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Issue.html#-i6gy7u_4)*

- `id` String

- `attachments` Array of [IssueAttachments](#issue-attachments)
- `comments` Array of [IssueComments](#issue-comments)
- `commentsCount` Int

- `created` Long

- `customFields` Array of [IssueCustomFields](#issue-custom-fields)
- `description` String

- `draftOwner` [User](#user)

- `externalIssue` [ExternalIssue](#external-issue)

- `idReadable` String

- `isDraft` Boolean

- `links` Array of [IssueLinks](#issue-links)

- `numberInProject` Long

- `parent` [IssueLinks](#issue-link)

- `pinnedComments` Array of [IssueComments](#issue-comments)
- `project` [Project](#project)

- `reporter` [User](#user)

- `resolved` Long

- `subtasks` [IssueLink](#issue-link)

- `summary` String

- `tags` Array of [Tags](#tags)

- `updated` Long

- `updater` [User](#user)

- `visibility` [Visibility](#visibility)

- `voters` [IssueVoters](#issue-voters)

- `votes` Int

- ` watchers` [IssueWatchers](#issue-watchers)

- `wikifiedDescription` String
---


### <a id='issue-attachments'></a>IssueAttachments
*[see official IssueAttachments documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueAttachment.html)*

- `id` String

- `name` String

- `author` [User](#user)

- `created` Long

- `updated` Long

- `size` Long

- `extension` String

- `charset` String

- `mimeType` String

- `metaData` String

- `draft` Boolean

- `removed` Boolean

- `base64Content` String

- `url` String

- `visibility` [Visibility](#visibility)

- `issue` [Issue](#issue)

- `comment` [IssueComment](#issue-comment)

- `thumbnailURL` String
---

### <a id='issue-comments'></a>IssueComments
*[see official IssueComment documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueComment.html#-dk7yed_4)*

- `id` String

- `attachments` Array of [IssueAttachments](#issue-attachments)

- `author` [User](#user)

- `created` Long

- `deleted` Long

- `issue` [Issue](#issue)

- `pinned` Boolean

- `reactions` Array of [Reactions](#reaction)

- `text` String

- `textPreview` String

- `updated` Long

- `visibility` [Visibility](#visibility)

### <a id='reaction'></a>Reactions

### <a id='issue-custom-fields'></a>IssueCustomFields

### <a id='external-issue'></a>ExternalIssue



### <a id='issue-link'></a>IssueLink

### <a id='issue-voters'></a>IssueVoters

### <a id='issue-watchers'></a>IssueWatchers



### <a id='visibility'></a>Visibility

### <a id='tags'></a>Tags
