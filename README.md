# The YouTrack's Advanced Search and Data Extrapolation #  

**Introduction:**    

A work made by interns Adriano Rueda & Nicola Pucci from ITS Prodigi to bring value to the team.  

## Index #
- [Advanced Search](#advanced_search)
  - [Definition](#definition)  
  - [How to activate it](#how_to_activate_it)  
  - [Syntax](#syntax)  
  - [Search Suggestions](#search_suggestions)  
  - [Wildcards in Attribute-based Search](#wildcards)  
  - [Single Value Search](#single_value_search)  
  - [Searching for a Range of values](#searching_for_a_range_of_values)  
  - [Searching in Attributes that Store Text](#searching_in_attributes_that_store_text)  
  - [Query Completion](#query_completion)   
  - [Visual Indicators](#visual_indicators)
  
- [Data Extrapolation](#data_extrapolation)  
  - [Fields and Query Syntax](#fieldsandquerysyntax)  
  - [Pagination](#pagination)  
  - [Entity Attributes](#entityattributes)   
   - [User](#user)  
   - [Project](#project)  
   - [Issue](#issue)  

## <a id='advanced_search'></a> 1. Advanced Search ##

### <a id='definition'></a> 1.1 Definition ###
The YouTrack's Advanced search is the second option for searching issues in the platform. It allows you to search issues using attribute and value pairs.  

### <a id='how_to_activate_it'></a> 1.2 How to activate it  
To activate advanced search you have to select it in the settings button ![alt text](image.png) to the right of the search box. 

### <a id='syntax'></a> 1.3 Syntax ###
To compose an attribute-based search query there are various ways. Below that with examples:
- `:` to specify the value of an attribute
 ```
 project: IDEADEV
 # finds all issues in the project with name IDEADEV
```
- `#` to find issues with that value or state
```
#Open
# finds all issues with state Open
```
- `-` to exclude a value
```
 project: -IDEADEV
 # finds all issues where project name is *NOT* IDEADEV
 ```
- `..` to find issues between two values
```
project: IDEADEV Fixed in builds: 41238 .. 43006
# finds all issues in the project with name IDEADEV that were fixed in builds from 41238 to 43006
```
- `{}` to enclose values that contain spaces with braces
```
Type: {Support Request}
# finds all issues that are assigned the Support Request type
```
- `and` to include one or more issues to the original one 
```
project: IDEADEV State: -Fixed commenter: John Davis Type: {Support Request}
# finds support requests in the IDEADEV project that include comments from John Davis and are not assigned the Fixed state
```
- `or` to include an alternative issue 
```
project: IDEADEV and (Type: {Support Request} or commenter: John Davis)
# finds issues in the IDEADEV project that are either support requests or have a comment by John Davis
```

### <a id='search_suggestions'></a> 1.4 Search suggestions
Search suggestion can be made Smart o for User Accounts.  

Smart:  
The values suggested for subsequent attributes are restricted to the sets of values used in these projects are well when you search for a value only used in a specific set of projects. Un example occurs when you search for items that are assigned the `State: On hold`.

User Accounts:
Search attributes that reference users display usernames in the auto-completion list. These suggestions are only offered when you are granted the Read User Basic permissions in at least one project.

### <a id='wildcards'></a> 1.5 Wildcards in Attribute-based Search
You can use the `*` wildcards to find values in custom fields that store an `enum`, a `state`, an `ownedField`, a `version`, a `build` or a `string`. You can also use that for `sprint names`, `project names` and `tags`. Wildcards are not supported for fields that store a `user`, a `date`, a `period`, a `float` or an `integer`. The `?` wildcard is not supported in attribute-based search queries even for custom fields that store a `string`. 

### <a id='single_value_search'></a> 1.6 Single Value Search 
YouTrack allows to search for single values replacing a number sign or a minus in front of the value to put.  
Some example:
- `#{In Progress} -{Support Request}` finds all issues that are in progress that are not support requests;
- `Pricing: {-100} .. {-500}` finds issues with pricing values between -100 and -500;
- `#me -Resolved` finds issues that you reported, assigned or commented to that are unresolved.

### <a id='searching_for_a_range_of_values'></a> 1.7 Searching for a Range of Values
Like YouTrack allows you to search for single values allows you also to search for a Range of Values.  
To search for a range:  
1. Enter the name of the attribute followed by a `:`;
2. Enter the value that defines the upper or lower bound of the range followed by `..`;
3. Enter the value that closes the range. use the `*` as a wildcard.
4. Execute the search query.  

Some Query of Search for a Range of Values with closed like Range:
1. `Spent time: 1h .. 2h` returns issues in wich amount of spent time is at least one hour but not greater than two hours.
2. `Created: 2018-03-10 .. 2018-03-13` 	
returns issues created from March 10 to March 13, 2018. 
3. `Actual start: 2025-03-25T09:00 .. 2025-03-25T18:00` returns issues started on March 25, 2025 between 09:00 and 18:00.
4. `Story Points: 1 .. 5` returns issues that are assigned at least one story point but not more than five story points.
5. `Price: {-100} .. {-500}` returns issues that have a price value between -100 and -500.
6. `Type: Bug #Resolved Fixed in builds: 41238 .. 43006` returns resolved issues that are categorized as bugs that are fixed in builds from 41238 to 43006.  

Now with open like Range:  
1. `Spent time: 8h1m .. *` returns issues in which the amount of spent time is greater than one day.
2. `Created: 2018-03-10 .. 2018-03-13` returns issues created on or before March 10, 2018.
3. `Actual start: 2025-03-25T014:00 .. *` returns issues started on or after March 25, 2025 at 14:00.
4. `Story Points: 10 .. *` returns issues that are assigned 10 story points and higher.
5. `Price: {-500} .. *` returns issues 
have a price value of -500 or higher.
6. `votes: 30 .. * #Unresolved` returns unresolved issues that have 30 or more votes.
7. `Priority: * .. Major #Unresolved` returns unresolved issues that are assigned major priority or higher.
8. `Type: Bug #Resolved Fixed in builds: 41238 .. *` returns resolved issues that are categorized as bugs that are fixed in builds 41238 and higher.

### <a id='searching_in_attributes_that_store_text'></a> 1.8 Searching in Attributes that Store Text ###
Attributes like the issue summary, description or comments are stored as text. When you specify one of these attributes in your search query YouTrack understands that you want to check the specified attribute for one or more words. Consequentially YouTrack searches inside the attribute for matches to the specified text. The syntax between text search and attribute-based search is slightly different. In this kind of search you can enter as many words as you want, in any order.  See it with some queries:

1. `project: IDEADEV commenter: John Davis description: customer support`
returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description.
2. `project: IDEADEV commenter: John Davis description: "customer support"`
returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description as a phrase.
### <a id='query_completion'></a> 1.9 Query Completion
The search box supports query completion. This feature suggests possible attributes and values that are based on your current input. When you start to enter in an empty search query YouTrack suggests attributes and values for keywords that match with the current input. You can select that keyword from the list using your mouse or keyboard.
There are two modes of operation:  
1. If you select an attribute the query completion suggests possible values for the selected attribute.
2. If you select a single value that value is added to the query.  
### <a id='visual_indicators'></a> 1.10 Visual Indicators
When you enter a query in the search box YouTrack applies visual indicators to the query. This formatting tells you that your query has been parsed as a structured search.  
Be careful to some warnings:
1. Issue attributes are shown as normal text without formatting.
2. Attribute values are shown in blue text.
3. Words or phrases that are parsed as a text search are shown in gold.
4. Invalid parameters are underlined in red.
## <a id='data_extrapolation'></a> 2. Data Extrapolation



### YouTrack REST Api

#### notes
- header `Authorization: 'Bearer {your_token}'` is required for every request

- use **Accept** HTTP header to indicate the expected response data: `application/json`

- enpoint are exposed at: `{your_youtrack_url}/api`

- for POST and PUT use **Content-Type** `application/json` 

- YouTrack supports `OAuth2.0` but prefers the usage of a [permanent token](https://www.jetbrains.com/help/youtrack/devportal/authentication-with-permanent-token.html)

- eats and returns json data

- you can add custom endpoints


### Fields and Query Syntax

in YouTrack REST Api, when you send a request to retrieve a resource, by default the server sends back only the database ID and the `$type` of the resource entity.
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
### Data obtained via URL
To retrieve data via URL:  
https://{HOST:PORT}{url}


#### Example: retrieve attachments
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


### Pagination
The server by default retorn max 50 entries.
To work with pagination use `$top` and `$skip`
**example**:
```
{base_url}/api/issues/?fields=id,summary&$top=40&$skip=80
```
the example above will skip the first 80 results and return results between 81 and 120 (if they exist)



## Entity Attributes

### User 
*[see official User documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-User.html#x7x140_6)*
- `id` String

- `login` String `not needed`

- `fullName` String

- `email` String `can be null` `not needed`

- `ringId` String You can use this ID for operations in Hub and for matching users between YouTrack and Hub. `can be null` `not needed`

- `guest` Boolean `not needed`

- `online` Boolean `not needed`

- `banned` Boolean `not needed`

- `tags` Array of [Tags](#tags)

- `savedQueries` Array of [savedQueries](#savedqueries) 

- `avatarUrl` String  `not needed`

- `profiles` [UserProfiles](#userprofiles) `not needed`

### UserProfiles `not needed`
*[see official UserProfiles documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-UserProfiles.html#eepffu_2)*

- `id` String

- `general` [GeneralUserProfile](#generaluserprofile)

- `notifications` [NotificationsUserProfile](#notificationsuserprofile)

- `timetracking` [TimeTrackingUserProfile](#timetrackinguserprofile)


### GeneralUserProfile
*[see official GeneralUserProfile documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-GeneralUserProfile.html)*

- `id` String

- `dateFieldFormat` [DateFormatDescriptor](#dateformatdescriptor)

- `timezone` [TimeZoneDescriptor](#timezonedescriptor)

- `locale` [LocaleDescriptor](#localedescriptor)

### DateFormatDescriptor
*[see official DateFormatDescriptor decumentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-DateFormatDescriptor.html)*

- `id` String

- `presentation` String (Name of the date format)

- `pattern` String (pattern applied when showing date with time)

- `datePattern` String

### TimeZoneDescriptor
*[see official TimeZoneDescription documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-TimeZoneDescriptor.html)*

- `id` String

- `presentation` String

- `offset` Int

### LocaleDescriptor
*[see official LocaleDescriptor documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-LocaleDescriptor.html)*

- `id` String

- `locale` String (locale code, `can be null`)

- `language` String (language abbreviation, `can be null`) 

- `community` Boolean (Indicated whether this locale is supported by community. if `false` the locale is official translation)

- `name` String `can be null`


### TimeTrackingUserProfile
*[see official TimeTrackingUserProfile documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-TimeTrackingUserProfile.html)*

- `id` String

- `perdiodFormat` [PerdiodFieldFormat](#periodfieldformat)

### PeriodFieldFormat
*[see official PeriodFieldFormat documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-PeriodFieldFormat.html#-p6sn5v_2)*

- `id` String (available formats are `FULL`(week,day,howe,minutes) - `DAYS_HOURS_MINUTES` - `HOURS_MINUTES` - `MINUTES`)


### NotificationsUserProfile
*[see official NotificationsUserProfile documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-NotificationsUserProfile.html)*

- `id` String

- `notifyOnOwnChanges` Boolean

- `emailNotificationsEnabled` Boolean

- `mentionNotificationsEnabled` Boolean

- `duplicateClusterNotificationsEnabled` Boolean

- `mailboxIntegrationNotificationsEnabled` Boolean

- `usePlainTextEmails` Boolean

- `autoWatchOnComment` Boolean

- `autoWatchOnCreate` Boolean

- `autoWatchOnVote` Boolean

- `autoWatchOnUpdate` Boolean

### UserGroup
*[see official UserGroup documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-UserGroup.html)*

- `id` String

- `name` String

- `ringId` String Use this ID for operations in Hub, and for matching groups between YouTrack and Hub. `can be null`

- `usersCount` Long

- `icon` String `can be null`

- `allUsersGroup` Boolean

- `teamForProject` [Project](#project) `can be null`



### Project
*[see official Project documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Project.html)*

- `id` String  

- `archived` Boolean  

- `createdBy` [User](#user)  

- `customFields` [ProjectCustomField](#projectcustomfield)   

- `description` String  

- `fromEmail` String  

- `iconUrl` String  

- `issues` Array of [Issues](#issue)  

- `leader` [User](#user)  

- `name` String  

- `replyToEmail` String   

- `shortName` String  

- `startingNumber` Long  

- `team` [projectTeam](#projectteam)  

- `template` Boolean


### ProjectTeam
*[see official ProjectTeam documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-ProjectTeam.html)

- `project` [Project](#project)

- `id` String

- `name` String

- `ringId` String

- `usersCount` Long

- `icon` String

- `allUsersGroup` Boolean

- `teamForProject` [Project](#project)


### Issue
*[see official Issue documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Issue.html#-i6gy7u_4)*

- `id` String

- `attachments` Array of [IssueAttachments](#issueattachments)

- `comments` Array of [IssueComments](#issuecomments)

- `commentsCount` Int

- `created` Long 

- `customFields` Array of [IssueCustomFields](#issuecustomfields)

- `description` String  `can be null`

- `draftOwner` [User](#user)  (creator of the draft if the issue is a draft. `null` if the issue is reported. `can be null`)

- `externalIssue` [ExternalIssue](#externalissue)  `can be null`

- `idReadable` String

- `isDraft` Boolean

- `links` Array of [IssueLinks](#issuelinks)

- `numberInProject` Long

- `parent` [IssueLinks](#issuelink) `can be null`

- `pinnedComments` Array of [IssueComments](#issuecomments)

- `project` [Project](#project) `can be null`

- `reporter` [User](#user) `can be null`

- `resolved` Long `can be null`

- `subtasks` [IssueLink](#issuelink)

- `summary` String

- `tags` Array of [Tags](#tags) `can be null`

- `updated` Long

- `updater` [User](#user) `can be null`

- `visibility` [Visibility](#visibility) `can be null`

- `voters` [IssueVoters](#issuevoters)

- `votes` Int

- ` watchers` [IssueWatchers](#issuewatchers)

- `wikifiedDescription` String



### IssueAttachments
*[see official IssueAttachments documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueAttachment.html)*

- `id` String

- `name` String

- `author` [User](#user) `can be null`

- `created` Long

- `updated` Long

- `size` Long

- `extension` String `can be null`

- `charset` String `can be null`

- `mimeType` String `can be null`

- `metaData` String `can be null`

- `draft` Boolean

- `removed` Boolean

- `base64Content` String (`can be null`. Data URI that represents the attachment with the syntax: data:[<media type>][;base64],<data>.  
For example: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==".)

- `url` String `can be null`

- `visibility` [Visibility](#visibility) `can be null`

- `issue` [Issue](#issue) `can be null`

- `comment` [IssueComment](#issuecomment)

- `thumbnailURL` String `can be null`


### IssueComments
*[see official IssueComment documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueComment.html#-dk7yed_4)*

- `id` String

- `attachments` Array of [IssueAttachments](#issueattachments)

- `author` [User](#user) `can be null`

- `created` Long

- `deleted` Boolean

- `issue` [Issue](#issue) `can be null`

- `pinned` Boolean

- `reactions` Array of [Reactions](#reaction)

- `text` String `can be null`

- `textPreview` String

- `updated` Long `can be null`

- `visibility` [Visibility](#visibility) `can be null`


### Reactions
*[see official Reactions documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Reaction.html)*

- `id` String

- `author` [User](#user) `can be null`

- `reaction` String `can be null`


### IssueCustomFields
*[see official IssueCustomFields documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueCustomField.html)*

- `id` String

- `name` String

- `projectCustomField` [ProjectCustomField](#projectcustomfield)

- `value` **Doesn't have a fixed Type** it can also be a single value or an Array

### ProjectCustomField
*[see official ProjectCustomField documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-ProjectCustomField.html)*

- `id` String

- `field` [CustomField](#customfield) `can be null'

- `project` [Project](#project) `can be null`

- `canBeEmpty` Boolean

- `emptyFieldText` String `can be null`

- `ordinal` Int

- `isPublic` Boolean

- `hasRunningJob' Boolean (if `true` there is a backgound job running for this field. In this case some field operations may be temporary inaccessible)

- `condition` [CustomFieldCondition] (condition for showing the field `can be null`)

### CustomField
*[see official CustomField documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-CustomField.html)*
 - `id` String
 
 - `name` String
 
 - `localizedName` string `can be null`
 
 - `fieldType` [FieldType](#fieldtype)
 
 - `isAutoAttached` Boolean (if `true` the field will be automatically attached to new projects)
 
 - `isDisplayedIsIssueList` Boolean
 
 - `ordinal` Int (number of the field)
 
 - `aliases` String `can be null`
 
 - `fieldDefaults` [CustomFieldDefaults]
 
 - `hasRunningJob` Boolean
 
 - `isUpdateable` Boolean
 
 - `instances` Arrat of [ProjectCustomFields](#projectcustomfields)
 

### CustomFieldDefault
*[see official CustomFieldDefault documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-CustomFieldDefaults.html)*

- `id` String

- `canBeEmpty` Boolean

- `emptyFieldText` String `can be null`

- `isPublic` Boolean

- `parent` [CustomField](#customfield)


### CustomFieldCondition
*[see official CustomFieldCondition documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-CustomFieldCondition.html)*

- `id` String

- `parend` [ProjectCustomField](#projectcustomfield) (the custom field that is hidden behind the condition. `can be null`)


### FieldType
*[see official FieldType documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-FieldType.html)

- `id` String (to see all available field types see [Type Mapping for Custom Fields](https://www.jetbrains.com/help/youtrack/devportal/api-concept-custom-fields.html#type-issue-custom-fields))



### ExternalIssue
*[see official ExternalIssue documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-ExternalIssue.html)*

- `id` String

- `name` String

- `url` String `can be null`

- `key` String `can be null`


### IssueLink
*[see official IssueLink documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueLink.html#frih9c_4)*

- `id` String

- `direction` Enumeration of: OUTWARD,INWARD and BOTH

- `linkType` [IssueLinkType](#issuelinktype) `can be null`

- `issues` Array of [Issues](#issue)

- `trimmedIssues` Array of [Issues](#issue)



### IssueLinkType
*[see official IssueLinkType documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueLinkType.html)*

- `id` String

- `name` String

- `localizedName` String `can be null`

- `sourceToTarget` String

- `localizedSourceToTarget` String `can be null`

- `targetToSource` String `can be null`

- `localizedTargetToSource` String

- `directed` Boolean

- `aggregation` Boolean

- `readOnly` Boolean

### IssueVoters
*[see official IssueVoters documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueVoters.html#jhr89d_2)

- `id` String

- `hasVote` Boolean

- `original` Array of [Users](#user)

- `duplicate` Array of [DuplicateVotes](#duplicatevotes)

### DuplicateVote
*[see official DuplicateVote documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-DuplicateVote.html)

- `id` String
 
- `issue` [Issue](#issue) `can  be null`

- `user` [User](#user) `can  be null`


### IssueWatchers
*[see official IssueWatchers documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-IssueWatchers.html#-koi4gz_2)*

- `id` String

- `hasStar` Boolean

- `issueWatchers` Array of *IssueWatchers*

- `duplicateWatchers` Array of *IssueWatchers*


### Visibility
*[see official Visibility documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Visibility.html)*  
*Visibility has no attributes, but is extended by `UnlimitedVisibility` and `LimitedVisibility`*
#### UnlimitedVisibility
UnlimitedVisibility has no attributes.
#### LimitedVisibility
 - `id` String
 - `permittedGroups` Array of [UserGroups](#usergroup)
 - `permittedUsers` Array of [Users](#user)

### Tags
*[see official Tag documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-Tag.html#-t2ffi1_7)*

- `id` String

- `issues` Array of [Issues](#issue)

- `color` [FieldStyle](#fieldstyle)

- `untagOnresolve` Boolean

- `visibleFor` [UserGroup](#usergroup) `can be null`

- `updateableBy` [UserGroup](#usergroup) `can  be null`

- `readSharingSettings` [WatchFolderSharingSettings](#watchfoldersharingsettings)

- `tagSharingSettings` [TagSharingSettings](#tagsharingsettings)

- `updateSharingSettings` [WatchFolderSharingSettings](#watchfoldersharingsettings)

- `owner` [User](#user)

- `name` String


### FieldStyle
*[see official FieldStyle documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-FieldStyle.html)*

- `id` String

- `background` String `can  be null`

- `foreground` String `can  be null`


### WatchFolderSharingSettings
*[see official WatchFolderSharingSettings documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-WatchFolderSharingSettings.html)*

- `id` String

- `permittedGroups` Array of [UserGroups](#usergroup)

- `permittedUsers` Array of [Users](#user)


### TagSharingSettings
*[see official TagSharingSettings documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-TagSharingSettings.html)*

- `id` String

- `permittedGroups` Array of [UserGroups](#usergroup)

- `permittedUsers` Array of [userGroups](#usergroup)

### SavedQueries
*[see official SavedQueries documentation](https://www.jetbrains.com/help/youtrack/devportal/api-entity-SavedQuery.html#z79p1qu_5)

- `id` String

- `query` String `can be null`

- `issues` Array of [Issues](#issue)

- `visibleFor` [UserGroup](#usergroup) `can be null`

- `updateableBy` [UserGroup](#usergroup) `can be null`

- `readSharingSettings` [WatchFolderSharingSettings](#watchfoldersharingsettings)

- `updateSharingSettings` [WatchFolderSharingSettings](#watchfoldersharingsettings)

- `owner` [User](#user) `can be null`

- `name` String
