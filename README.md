The YouTrack's Advanced Search and Data Extrapolation
A summary of the YouTrack's official documentation.
Made by Adriano Rueda & Nicola Pucci

Index
Advanced Search

Introduction
Definition
How to activate it
Syntax
Search Suggestions
Wildcards in Attribute-based Search
Single Value Search
Searching for a Range of values
Searching in Attributes that Store Text
Query Completion
Visual Indicators
REST API & Apps

Introduction
YouTrack REST API
Fields and Query Syntax
Fields
Custom Fields
Query
Data obtained via URL
Pagination
Apps
HTTP Handler
Rules
Entity Attributes
User
Project
Issue
Our Final Thoughts

1. Advanced Search
1.1 Introduction
What is the advanced search and how it works compared to the simple search.

1.2 Definition
The YouTrack's Advanced search is the second option for searching issues in the platform. It allows you to search issues using attribute and value pairs.

1.3 How to activate it
To activate advanced search you have to select it in the settings button alt text to the right of the search box.

1.4 Syntax
To compose an attribute-based search query there are various ways. Below that with examples:

: to specify the value of an attribute
project: IDEADEV
# finds all issues in the project with name IDEADEV
# to find issues with that value or state
#Open
# finds all issues with state Open
- to exclude a value
 project: -IDEADEV
 # finds all issues where project name is *NOT* IDEADEV
.. to find issues between two values
project: IDEADEV Fixed in builds: 41238 .. 43006
# finds all issues in the project with name IDEADEV that were fixed in builds from 41238 to 43006
{} to enclose values that contain spaces with braces
Type: {Support Request}
# finds all issues that are assigned the Support Request type
and to include one or more issues to the original one
project: IDEADEV State: -Fixed commenter: John Davis Type: {Support Request}
# finds support requests in the IDEADEV project that include comments from John Davis and are not assigned the Fixed state
or to include an alternative issue
project: IDEADEV and (Type: {Support Request} or commenter: John Davis)
# finds issues in the IDEADEV project that are either support requests or have a comment by John Davis
1.5 Search suggestions
Search suggestion can be made Smart o for User Accounts.

Smart:
The values suggested for subsequent attributes are restricted to the sets of values used in these projects are well when you search for a value only used in a specific set of projects. Un example occurs when you search for items that are assigned the State: On hold.

User Accounts: Search attributes that reference users display usernames in the auto-completion list. These suggestions are only offered when you are granted the Read User Basic permissions in at least one project.

1.6 Wildcards in Attribute-based Search
You can use the * wildcards to find values in custom fields that store an enum, a state, an ownedField, a version, a build or a string. You can also use that for sprint names, project names and tags. Wildcards are not supported for fields that store a user, a date, a period, a float or an integer. The ? wildcard is not supported in attribute-based search queries even for custom fields that store a string.

1.7 Single Value Search
YouTrack allows to search for single values replacing a number sign or a minus in front of the value to put.
Some example:

#{In Progress} -{Support Request} finds all issues that are in progress that are not support requests;
Pricing: {-100} .. {-500} finds issues with pricing values between -100 and -500;
#me -Resolved finds issues that you reported, assigned or commented to that are unresolved.
1.8 Searching for a Range of Values
Like YouTrack allows you to search for single values allows you also to search for a Range of Values.
To search for a range:

Enter the name of the attribute followed by a :;
Enter the value that defines the upper or lower bound of the range followed by ..;
Enter the value that closes the range. use the * as a wildcard.
Execute the search query.
Some Query of Search for a Range of Values with closed like Range:

Spent time: 1h .. 2h returns issues in wich amount of spent time is at least one hour but not greater than two hours.
Created: 2018-03-10 .. 2018-03-13 returns issues created from March 10 to March 13, 2018.
Actual start: 2025-03-25T09:00 .. 2025-03-25T18:00 returns issues started on March 25, 2025 between 09:00 and 18:00.
Story Points: 1 .. 5 returns issues that are assigned at least one story point but not more than five story points.
Price: {-100} .. {-500} returns issues that have a price value between -100 and -500.
Type: Bug #Resolved Fixed in builds: 41238 .. 43006 returns resolved issues that are categorized as bugs that are fixed in builds from 41238 to 43006.
Now with open like Range:

Spent time: 8h1m .. * returns issues in which the amount of spent time is greater than one day.
Created: 2018-03-10 .. 2018-03-13 returns issues created on or before March 10, 2018.
Actual start: 2025-03-25T014:00 .. * returns issues started on or after March 25, 2025 at 14:00.
Story Points: 10 .. * returns issues that are assigned 10 story points and higher.
Price: {-500} .. * returns issues have a price value of -500 or higher.
votes: 30 .. * #Unresolved returns unresolved issues that have 30 or more votes.
Priority: * .. Major #Unresolved returns unresolved issues that are assigned major priority or higher.
Type: Bug #Resolved Fixed in builds: 41238 .. * returns resolved issues that are categorized as bugs that are fixed in builds 41238 and higher.
1.9 Searching in Attributes that Store Text
Attributes like the issue summary, description or comments are stored as text. When you specify one of these attributes in your search query YouTrack understands that you want to check the specified attribute for one or more words. Consequentially YouTrack searches inside the attribute for matches to the specified text. The syntax between text search and attribute-based search is slightly different. In this kind of search you can enter as many words as you want, in any order. See it with some queries:

project: IDEADEV commenter: John Davis description: customer support returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description.
project: IDEADEV commenter: John Davis description: "customer support" returns all issues in the IDEADEV project that include comments from John Davis and contain forms of the words "customer" and "support" in the description as a phrase.
1.10 Query Completion
The search box supports query completion. This feature suggests possible attributes and values that are based on your current input. When you start to enter in an empty search query YouTrack suggests attributes and values for keywords that match with the current input. You can select that keyword from the list using your mouse or keyboard. There are two modes of operation:

If you select an attribute the query completion suggests possible values for the selected attribute.
If you select a single value that value is added to the query.
1.11 Visual Indicators
When you enter a query in the search box YouTrack applies visual indicators to the query. This formatting tells you that your query has been parsed as a structured search.
Be careful to some warnings:

Issue attributes are shown as normal text without formatting.
Attribute values are shown in blue text.
Words or phrases that are parsed as a text search are shown in gold.
Invalid parameters are underlined in red.
2. REST API & Apps
2.1 Introduction
How data from Youtrack are exported related to API.

2.2 YouTrack REST Api
What to know...

header Authorization: 'Bearer {your_token}' is required for every request

use Accept HTTP header to indicate the expected response data: application/json

enpoint are exposed at: {your_youtrack_url}/api

for POST and PUT use Content-Type application/json

YouTrack supports OAuth2.0 but prefers the usage of a permanent token.

You can add custom endpoints

2.3 Fields and Query Syntax
in YouTrack REST Api, when you send a request to retrieve a resource, by default the server sends back only the database ID and the $type of the resource entity. To receive attributes in response from the server you must explicitly specify the fields parameter.

2.4 Fields
fields allows you to choose the attributes you get the response.
Here's an example where we only want id, summary and project name of the issue:

https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)
the server will seturn something like:

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
2.5 Custom Fields
CustomField is an entity that defines a common custom field wich contains basic attributs like name and fieldType.

If you want to see the custom fields of the issues you have to include customFields in the fields, you also have to specify which of the customField's attribute you want in your response

{your_url}/api/issues/?fields=id,summary,customFields(id,projectCustomField(field(name)),value(name))
2.6 Query
! You can only use queries in GET requests that work with collections of issues or tags !

query allows you to filter by the value of the attribute.
The syntax for the queries is almost the same as Advanced Search.
Here's an example where we want to see the issues of the project Sample Project.

https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=project:+%7BSample+Project%7D

the response body will contain data only for the matching issues

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
Here is a bit more complex query. we are looking for issues that are assigned to the user 'john.doe', not yet resolver, and contain the word 'summary'.

https://example.youtrack.cloud/api/issues?fields=id,summary,project(name)&query=for:+john.doe+%23Unresolved+summary
2.7 Data obtained via URL
To retrieve data via URL:
https://{HOST:PORT}{url}

Example: retrieve attachments
To get the attachments of a specific issue we need the attachment's url.
To get the urls we'll take the list of attachments of the issue

/api/issues/{issue_id}/attachments
the response will look like this:

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
now we can use the url in the attachment to receive the attachment.

https://{HOST:PORT}{url_in_attachments}
Note:
we don't need to include the token in the header of the request to retrieve the attachments because the client is identified using the sign= parameter in the url received from /api/issues/{issue_id}/attachments.

If we want to take all the attachments from the issues of the Project MioProject:

{base_url}/api/issues?fields=id,attachments(url)&query=project:MioProject
This will return a list of issues, each issue will have id and a list of attachments and each attachment will have his url

2.8 Pagination
The server by default retorn max 50 entries. To work with pagination use $top and $skip example:

{base_url}/api/issues/?fields=id,summary&$top=40&$skip=80
the example above will skip the first 80 results and return results between 81 and 120 (if they exist)

2.9 Apps
Check app quick start guide for more.

Apps in Youtrack allow you to customize your youtrack. One of the customizations you can do is implementing modules,
modules can be mainly rules or HTTP Handlers.

Could not confirm with Official documentation
It seems that script have a limit on execution time and memory usage. (According to some LLM we are talking abt 30-60 sec execution time and in terms of issues about 2000)

To start creating an app run:

npm build @jetbrains/youtrack-app@latest
Follow the prompts provided by the generator.

now you can customize your app, remember that modules are js files.

When your app is ready, run:

npm run build
then

npm run upload -- --host <Your_Url> --token <You_Token>
If you rather add it directly on YouTrack, go to app, in the top-right corner there will be an add app button, press it then press Upload .zip

HTTP Handlers
Your HTTP Handler can access data via YouTrack WebApi or the internal YouTrack Apis.

here's an example of a HTTP Handler

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
Rules
Rules can do a wide variety of things, from validating a value change in a custom type to perfoming an scheduled action.
The main types of rules are on-change. on-schedule, state-machine, action.

2.10 Entity Attributes
(each one can have a division into necessary and not necessary to distinguish the attributes while all are represented by a Field and a Type plus a description if present)

User
click here to see his official documentation

Attributes necessary:

id String
fullName String
tags Array of Tags
savedQueries Array of savedQueries
Attributes not necessary:

login String
email String. Can be null
ringId String. Usable for operations in Hub and for matching users between YouTrack and the first one. Can be null
guest Boolean
online Boolean
banned Boolean
avatarUrl String
profiles UserProfiles(see down)
UserProfiles (not always needed)
click here to see his official documentation

id String

general GeneralUserProfile

notifications NotificationsUserProfile

timetracking TimeTrackingUserProfile

GeneralUserProfile
click here to see his official documentation

id String

dateFieldFormat DateFormatDescriptor

timezone TimeZoneDescriptor

locale LocaleDescriptor

DateFormatDescriptor
click here to see his official documentation

id String

presentation String. Represents the name of the date format

pattern String. Is applied when showing date with time

datePattern String

TimeZoneDescriptor
click here to see his official documentation

id String

presentation String

offset Int

LocaleDescriptor
click here to see his official documentation

id String

locale String. Locale code. Can be null

language String. Language abbreviation. Can be null

community Boolean. Indicates whether this locale is supported by community. If false the locale is official translation.

name String. Can be null

TimeTrackingUserProfile
click here to see his official documentation

id String

perdiodFormat PerdiodFieldFormat

PeriodFieldFormat
click here to see his official documentation

id String. The ID of the period format. The available formats are FULL(number of weeks,days,hours and minutes), DAYS_HOURS_MINUTES(number of days, hours and minutes), HOURS_MINUTES(number of hours and minutes) or MINUTES(number of minutes))
NotificationsUserProfile
click here to see his official documentation

id String

notifyOnOwnChanges Boolean

emailNotificationsEnabled Boolean

mentionNotificationsEnabled Boolean

duplicateClusterNotificationsEnabled Boolean

mailboxIntegrationNotificationsEnabled Boolean

usePlainTextEmails Boolean

autoWatchOnComment Boolean

autoWatchOnCreate Boolean

autoWatchOnVote Boolean

autoWatchOnUpdate Boolean

UserGroup
click here to see his official documentation

id String

name String

ringId String. Usable for operations in Hub and for matching groups between YouTrack and the first one. Can be null

usersCount Long

icon String. Can be null

allUsersGroup Boolean

teamForProject Project. Can be null

Project
click here to see his official documentation

id String

archived Boolean

createdBy User

customFields ProjectCustomField

description String

fromEmail String

iconUrl String

issues Array of Issues

leader User

name String

replyToEmail String

shortName String

startingNumber Long

team projectTeam

template Boolean

ProjectTeam
click here to see his official documentation

project Project

id String

name String

ringId String

usersCount Long

icon String

allUsersGroup Boolean

teamForProject Project

Issue
click here to see his official documentation

id String

attachments Array of IssueAttachments

comments Array of IssueComments

commentsCount Int

created Long

customFields Array of IssueCustomFields

description String. Can be null

draftOwner User. Shows the creator of the draft if the issue is a draft. Shows null if the issue is reported

externalIssue ExternalIssue. Can be null

idReadable String

isDraft Boolean

links Array of IssueLinks

numberInProject Long

parent IssueLinks. Can be null

pinnedComments Array of IssueComments

project Project. Can be null

reporter User. Can be null

resolved Long. Can be null

subtasks IssueLink

summary String

tags Array of Tags. Can be null

updated Long

updater User. Can be null

visibility Visibility. Can be null

voters IssueVoters

votes Int

watchers IssueWatchers

wikifiedDescription String

IssueAttachments
click here to see his official documentation

id String

name String

author User. Can be null

created Long

updated Long

size Long

extension String. Can be null

charset String. Can be null

mimeType String. Can be null

metaData String. Can be null

draft Boolean

removed Boolean

base64Content String. Can be null. Is the content showed as Data URI that represents the attachment with the syntax: data:[][;base64],.
For example: "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==". Can be null

url String. Can be null

visibility Visibility. Can be null

issue Issue. Can be null

comment IssueComment

thumbnailURL String. Can be null

IssueComments
click here to see his official documentation

id String

attachments Array of IssueAttachments

author User. Can be null

created Long

deleted Boolean

issue Issue. Can be null

pinned Boolean

reactions Array of Reactions

text String. Can be null

textPreview String

updated Long. Can be null

visibility Visibility. Can be null

Reactions
click here to see his official documentation

id String

author User. Can be null

reaction String. Can be null

IssueCustomFields
click here to see his official documentation

id String

name String

projectCustomField ProjectCustomField

value Is assigned to the custom field in the issue. Depending on the type of the field it can store a single value or an array of values.

ProjectCustomField
click here to see his official documentation

id String

field CustomField. Can be null

project Project. Can be null

canBeEmpty Boolean

emptyFieldText String. Can be null

ordinal Int

isPublic Boolean

hasRunningJob Boolean. If is true there is a background job running for this field but some field's operations may be temporary inaccessible

condition CustomFieldCondition. Condition for showing the field can be null

CustomField
click here to see his official documentation

id String

name String

localizedName String. Can be null

fieldType FieldType

isAutoAttached Boolean. If is true the field will be automatically attached to new projects

isDisplayedIsIssueList Boolean

ordinal Int

aliases String. Can be null

fieldDefaults CustomFieldDefaults

hasRunningJob Boolean

isUpdateable Boolean

instances Arrat of ProjectCustomFields

CustomFieldDefault
click here to see his official documentation

id String

canBeEmpty Boolean

emptyFieldText String. Can be null

isPublic Boolean

parent CustomField

CustomFieldCondition
click here to see his official documentation

id String

parend ProjectCustomField. Represents the custom field that is hidden behind the condition. Can be null

FieldType
click here to see his official documentation

id String. Click here to see all types which he can takes
ExternalIssue
click here to see his official documentation

id String

name String

url String. Can be null

key String. Can be null

IssueLink
click here to see his official documentation

id String

direction Enumeration. Supports the values of OUTWARD,INWARD and BOTH

linkType IssueLinkType. Can be null

issues Array of Issues

trimmedIssues Array of Issues

IssueLinkType
click here to see his official documentation

id String

name String

localizedName String. Can be null

sourceToTarget String

localizedSourceToTarget String. Can be null

targetToSource String. Can be null

localizedTargetToSource String

directed Boolean

aggregation Boolean

readOnly Boolean

IssueVoters
click here to see his official documentation

id String

hasVote Boolean

original Array of Users

duplicate Array of DuplicateVotes

DuplicateVote
click here to see his official documentation

id String

issue Issue. Can be null

user User. Can be null

IssueWatchers
click here to see his official documentation

id String

hasStar Boolean

issueWatchers Array of IssueWatchers

duplicateWatchers Array of IssueWatchers

Visibility
click here to visit his official documentation.

Visibility has no attributes, but is extended by two kind: UnlimitedVisibility and LimitedVisibility.

UnlimitedVisibility
UnlimitedVisibility has not attributes.

LimitedVisibility
has like attributes:

id String
permittedGroups Array of UserGroups
permittedUsers Array of Users
Tags
click here to see his official documentation

id String

issues Array of Issues

color FieldStyle

untagOnresolve Boolean

visibleFor UserGroup. Can be null

updateableBy UserGroup. Can be null

readSharingSettings WatchFolderSharingSettings

tagSharingSettings TagSharingSettings

updateSharingSettings WatchFolderSharingSettings

owner User

name String

FieldStyle
click here to see his official documentation

id String

background String. Can be null

foreground String. Can be null

WatchFolderSharingSettings
click here to see his official documentation

id String

permittedGroups Array of UserGroups

permittedUsers Array of Users

TagSharingSettings
click here to see his official documentation

id String

permittedGroups Array of UserGroups

permittedUsers Array of userGroups

SavedQueries
click here to see his official documentation

id String

query String. Can be null

issues Array of Issues

visibleFor UserGroup. Can be null

updateableBy UserGroup. Can be null

readSharingSettings WatchFolderSharingSettings

updateSharingSettings WatchFolderSharingSettings

owner User. Can be null

name String

💡 Final Thoughts
A summary of the approach for data gathering and update:

Initial Idea (Straight-forward API):
Use the Web API for each entity to gather all data.
This requires specifying every desired field, which can lead to long lists, but it allows gathering all needed data.

Second Idea (HTTP Handlers for Pre-elaboration):
Use HTTP Handlers to pre-elaborate the data to make it easier to 'digest'.

Issue:
The scripting environment has limited memory and execution time (around 30-60 sec, 2000 issues limit).
The Workflow API lacks a pagination option, meaning some entities might be excluded if the count exceeds the maximum returned.

Final Thought (Hybrid Method):
Use a combination of all methods:

Initial Data Gathering: Use the first idea (Web API) with pagination.

Tracking Updates: Use on-Schedule or on-Change rules to track new or modified issues.

Optional Sending Updates: Use Web Api to send directly the modified issue's data to our backend, if backend responds with 201 remove issue's id from the tracked issue list.

Returning Updates: Use an HTTP Handler to return the issues tracked by the rules, or let the rule send the data of the new/modified issue directly to your server.

This approach is initially "expensive" but allows keeping the data updated.
A pagination option can be implemented in the Returning Updated phase
